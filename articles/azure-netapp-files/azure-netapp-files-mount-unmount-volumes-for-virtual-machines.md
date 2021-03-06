---
title: Zainstaluj woluminy Azure NetApp Files dla maszyn wirtualnych
description: Dowiedz się, jak zainstalować lub odinstalować wolumin dla maszyn wirtualnych z systemem Windows lub maszyn wirtualnych systemu Linux na platformie Azure.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.openlocfilehash: 4ad3800748330d5c3a6a32c6c0824bc72a05d0ef
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533091"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Instalowanie lub odinstalowywanie woluminu dla maszyn wirtualnych z systemem Windows lub Linux 

W razie potrzeby można zainstalować lub odinstalować wolumin dla maszyn wirtualnych z systemem Windows lub Linux.  Instrukcje instalacji dla maszyn wirtualnych z systemem Linux są dostępne na stronie Azure NetApp Files.  

> [!IMPORTANT] 
> Aby można było uzyskać dostęp do woluminu NFS, należy mieć co najmniej jedną zasadę eksportowania.

1. Kliknij blok **woluminy** , a następnie wybierz wolumin, dla którego chcesz zainstalować program. 
2. Kliknij pozycję **instrukcje instalacji** na wybranym woluminie, a następnie postępuj zgodnie z instrukcjami, aby zainstalować wolumin. 

    ![Instrukcje dotyczące instalacji systemu plików NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Instrukcje dotyczące instalacji SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)  
    * Jeśli instalujesz wolumin systemu plików NFS, upewnij się, że używasz `vers` opcji w `mount` poleceniu, aby określić wersję protokołu NFS odpowiadającą woluminowi, który chcesz zainstalować. 
    * Jeśli używasz NFSv 4.1, użyj następującego polecenia, aby zainstalować system plików:`sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Jeśli chcesz, aby wolumin NFS został automatycznie zainstalowany podczas uruchamiania lub ponownego uruchamiania maszyny wirtualnej platformy Azure, Dodaj wpis do `/etc/fstab` pliku na hoście. 

    Na przykład: `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP`jest adresem IP woluminu Azure NetApp Files znalezionym w bloku właściwości woluminu.
    * `$FILEPATH`jest ścieżką eksportu woluminu Azure NetApp Files.
    * `$MOUNTPOINT`to katalog utworzony na hoście z systemem Linux służący do instalowania eksportu systemu plików NFS.

4. Jeśli chcesz zainstalować wolumin w systemie Windows przy użyciu systemu plików NFS:

    a. Najpierw zainstaluj wolumin na maszynie wirtualnej z systemem UNIX lub Linux.  
    b. Uruchom `chmod 777` polecenie lub w `chmod 775` odniesieniu do woluminu.  
    c. Zainstaluj wolumin za pośrednictwem klienta NFS w systemie Windows.
    
5. Jeśli chcesz zainstalować wolumin systemu plików NFS Kerberos, zobacz [Konfigurowanie szyfrowania nfsv 4.1 Kerberos](configure-kerberos-encryption.md) , aby uzyskać dodatkowe szczegóły. 

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie domyślnej domeny NFSv4.1 dla usługi Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Często zadawane pytania dotyczące systemu NFS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
* [Omówienie systemu plików sieciowych](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview)
* [Instalowanie woluminu systemu plików NFS Kerberos](configure-kerberos-encryption.md#kerberos_mount)
