---
title: Verwenden von Azure Files mit AKS
description: "Verwenden von Azure-Datenträgern mit AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b6267dd2bc1b29229b2e8016e2429ed88b7bf676
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="using-azure-files-with-kubernetes"></a>Verwenden von Azure Files mit Kubernetes

Containerbasierte Anwendungen müssen häufig auf Daten in einem externen Datenvolume zugreifen und diese dauerhaft speichern. Azure-Dateien können als ein solcher externer Datenspeicher verwendet werden. In diesem Artikel wird erläutert, wie Azure-Dateien als Kubernetes-Volume in Azure Container Service verwendet werden.

Weitere Informationen zu Kubernetes-Volumes finden Sie unter [Kubernetes-Volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Bevor Sie eine Azure-Dateifreigabe als Kubernetes-Volume verwenden, müssen Sie ein Azure Storage-Konto und die Dateifreigabe erstellen. Das folgende Skript kann zum Ausführen dieser Aufgaben verwendet werden. Notieren oder aktualisieren Sie die Parameterwerte. Einige davon werden bei der Erstellung des Kubernetes-Volumes benötigt.

```azurecli-interactive
# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)
```

## <a name="create-kubernetes-secret"></a>Erstellen eines Kubernetes-Geheimnisses

Kubernetes benötigt Anmeldeinformationen für den Zugriff auf die Dateifreigabe. Diese Anmeldeinformationen werden in einem [Kubernetes-Geheimnis][kubernetes-secret] gespeichert, auf das bei der Erstellung eines Kubernetes-Pod verwiesen wird.

Wenn Sie ein Kubernetes-Geheimnis erstellen, müssen geheime Werte Base64-codiert sein.

Codieren Sie zuerst den Namen des Speicherkontos. Ersetzen Sie `$AKS_PERS_STORAGE_ACCOUNT_NAME` bei Bedarf durch den Namen Ihres Azure-Speicherkontos.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

Aktualisieren Sie dann den Schlüssel des Speicherkontos. Ersetzen Sie `$STORAGE_KEY` bei Bedarf durch den Namen des Azure-Speicherkontoschlüssels.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Erstellen Sie eine Datei namens „`azure-secret.yaml`“, und fügen Sie den folgenden YAML-Code ein. Aktualisieren Sie die Werte `azurestorageaccountname` und `azurestorageaccountkey` mit den Base64-codierten Werte, die Sie im letzten Schritt abgerufen haben.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Verwenden Sie den Befehl [kubectl create][kubectl-create] zum Erstellen des Geheimnisses.

```azurecli-interactive
kubectl create -f azure-secret.yaml
```

## <a name="mount-file-share-as-volume"></a>Einbinden einer Dateifreigabe als Volume

Sie können Ihre Azure Files-Freigabe in Ihren Pod einbinden, indem Sie das Volume in der entsprechenden Spezifikation konfigurieren. Erstellen Sie eine neue Datei namens „`azure-files-pod.yaml`“ mit folgendem Inhalt. Aktualisieren Sie „`aksshare`“ mit dem Namen der Azure Files-Freigabe.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Verwenden Sie „kubectl“, um einen Pod zu erstellen.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Sie haben jetzt einen aktiven Container, bei dem Ihre Azure-Dateifreigabe in das Verzeichnis `/mnt/azure` eingebunden ist. Sie können Ihre Volumeeinbindung anzeigen, wenn Sie Ihren Pod über `kubectl describe pod azure-files-pod` untersuchen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über Kubernetes-Volumes anhand von Azure Files.

> [!div class="nextstepaction"]
> [Kubernetes-Plug-In für Azure Files][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
