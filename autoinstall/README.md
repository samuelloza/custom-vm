# Instalación de Ubuntu mediante autoinstall/cloud-init con QEMU

Ejecuta `start-autoinstall.sh` para configurar el disco. (Este script invocará a `setup.sh`).

Después de ejecutar `setup.sh` —lo cual se hace automáticamente—:

1. Ejecuta `sudo cleanup.sh` en `/home/ansible/contestant-vm` con el usuario `ansible`.
2. Elimina la carpeta con:

   ```bash
   rm -rf /home/ansible/contestant-vm
   ```

3. Ejecuta:

   ```bash
   sudo shutdown now
   ```

> **Importante**: Para el paso de `zerofree`, deberás arrancar desde un disco en vivo ejecutando `boot-live-disk.sh` y luego correr:

```bash
zerofree /dev/sda2
```

> **NO inicies el disco después de ejecutar `cleanup.sh`**: el arranque después de ese script es especial. El script cloud-init no debe ejecutarse nuevamente para evitar que se cree una nueva identidad SSH en cada VM.

---

## Crear una imagen de VM

Convierte la imagen con:

```bash
qemu-img convert -f qcow2 -O vmdk disk.img contestant-vm.vmdk
```

Luego, copia `contestant-vm.vmdk` a una máquina virtual recién creada y correctamente configurada en VMWare.

---

## Crear una imagen nativa con Clonezilla

1. Inicia el servicio `sshd`:

```bash
sudo useradd -mU guest
sudo passwd guest
```

2. Arranca Clonezilla con:

```bash
start-clonezilla.sh
```

En el menú de Clonezilla selecciona:

- `Other modes of Clonezilla live`
- `Clonezilla live (KMS & To RAM)`

3. Elige:

- `device-image`
- `ssh_server`

Asegúrate de que haya un servidor DHCP y que Clonezilla monte:

```text
guest@10.0.2.2:/home/guest
```

La imagen será subida allí.

---

4. Selecciona el modo `beginner` y luego la opción `savedisk`.

Desde ahí, sigue las opciones predeterminadas para guardar la imagen del disco.
