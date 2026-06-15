# UNPi5_Image_Ubuntu24_04

**Autor:** Johan Alejandro López Arias ([@ElJoho](https://github.com/ElJoho))



<details>
  <summary>
    <b>Video tutorial</b> — <a href="https://www.youtube.com/watch?v=L0ZhnwBIGkc">Reestablecer sistema de raspberry pi 5 usando el backup</a>
  </summary>
</details>
<br>


Este repositorio contiene un backup del sistema de la Raspberry Pi 5 usado para restablecer la imagen del proyecto **UNAL PhantomX Pincher**.

El backup final corresponde a una imagen comprimida:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

Debido a su tamaño, el archivo fue dividido en **7 partes** y almacenado usando **Git LFS**.

Las partes se llaman:

```text
unpi5_PXP_ubuntu24_04.img.xz.part-001
unpi5_PXP_ubuntu24_04.img.xz.part-002
unpi5_PXP_ubuntu24_04.img.xz.part-003
unpi5_PXP_ubuntu24_04.img.xz.part-004
unpi5_PXP_ubuntu24_04.img.xz.part-005
unpi5_PXP_ubuntu24_04.img.xz.part-006
unpi5_PXP_ubuntu24_04.img.xz.part-007
```

> **Importante:** cada parte por separado **no** es un archivo `.xz` válido.
> Primero debes descargar correctamente las 7 partes y luego unirlas para reconstruir el archivo:
>
> ```text
> unpi5_PXP_ubuntu24_04.img.xz
> ```

No es necesario descomprimir manualmente el archivo `.img.xz`.
**Raspberry Pi Imager puede usar el archivo `.img.xz` directamente.**

---

## 1) Programas necesarios en Windows 11

Para restaurar el sistema desde este backup necesitas:

1. **Git for Windows / Git Bash**
   Se usa para clonar el repositorio desde GitHub.

2. **Git LFS**
   Se usa para descargar correctamente los archivos grandes del backup.

3. **CMD como administrador**
   Se usa para unir las 7 partes con el comando `copy /b`.

4. **Raspberry Pi Imager**
   Se usa para grabar la imagen `.img.xz` directamente en la microSD o SSD.

---

## 2) Clonar el repositorio correctamente con Git LFS

Abre **Git Bash** en Windows.

Primero entra a la carpeta donde quieres guardar el repositorio. Por ejemplo:

```bash
cd ~/path/backup_pi
```

Antes de clonar, activa Git LFS:

```bash
git lfs install
```

Luego clona el repositorio:

```bash
git clone https://github.com/labsir-un/UNPi5_Image_Ubuntu24_04.git
```

Entra a la carpeta del repositorio:

```bash
cd UNPi5_Image_Ubuntu24_04
```

Por seguridad, ejecuta:

```bash
git lfs pull
```

Este comando asegura que se descarguen los archivos reales de Git LFS y no solo los punteros pequeños.

---

## 3) Verificar que los archivos se descargaron correctamente

Desde Git Bash, revisa el tamaño de los archivos dentro de `ubuntuImage`:

```bash
ls -lh ubuntuImage/
```

También puedes entrar a la carpeta y revisar los archivos:

```bash
cd ubuntuImage
ls -lh
```

Cada parte debería pesar aproximadamente **1.3 GB**.

En el Explorador de Windows puede aparecer un tamaño similar a:

```text
1.300.000 KB
```

Eso significa aproximadamente **1.3 GB**.
El punto es separador de miles, no significa que el archivo pese solo 1 KB.

Si los archivos pesan solo unos pocos KB, entonces Git LFS no descargó el contenido real. En ese caso, vuelve a ejecutar desde la carpeta del repositorio:

```bash
git lfs pull
```

También puedes confirmar que los archivos no sean punteros de Git LFS abriendo uno de ellos con:

```bash
head -n 5 ubuntuImage/unpi5_PXP_ubuntu24_04.img.xz.part-001
```

Si aparece algo como esto:

```text
version https://git-lfs.github.com/spec/v1
oid sha256:...
size ...
```

entonces todavía tienes un puntero de Git LFS y no el archivo real.

Si el archivo pesa aproximadamente **1.3 GB** y no se muestra como texto legible, entonces la descarga está bien.

---

## 4) Unir las 7 partes usando CMD

Para unir las partes en Windows se debe usar **CMD**.

> No uses `copy /b` directamente en PowerShell.
> En PowerShell, `copy` es un alias de `Copy-Item`, por eso el comando puede fallar.

---

### 4.1 Abrir CMD como administrador

1. Abre el menú de inicio.
2. Busca `cmd`.
3. Haz clic derecho sobre **Símbolo del sistema**.
4. Selecciona **Ejecutar como administrador**.

---

### 4.2 Entrar a la carpeta `ubuntuImage`

En CMD, entra a la carpeta donde están las 7 partes.

Ejemplo:

```bat
cd /d "C:\Users\user_name\path\backup_pi\UNPi5_Image_Ubuntu24_04\ubuntuImage"
```

Verifica que las partes estén ahí:

```bat
dir
```

Debes ver estos archivos:

```text
unpi5_PXP_ubuntu24_04.img.xz.part-001
unpi5_PXP_ubuntu24_04.img.xz.part-002
unpi5_PXP_ubuntu24_04.img.xz.part-003
unpi5_PXP_ubuntu24_04.img.xz.part-004
unpi5_PXP_ubuntu24_04.img.xz.part-005
unpi5_PXP_ubuntu24_04.img.xz.part-006
unpi5_PXP_ubuntu24_04.img.xz.part-007
```

---

### 4.3 Unir las partes

Ejecuta el siguiente comando en **una sola línea**:

```bat
copy /b /y unpi5_PXP_ubuntu24_04.img.xz.part-001+unpi5_PXP_ubuntu24_04.img.xz.part-002+unpi5_PXP_ubuntu24_04.img.xz.part-003+unpi5_PXP_ubuntu24_04.img.xz.part-004+unpi5_PXP_ubuntu24_04.img.xz.part-005+unpi5_PXP_ubuntu24_04.img.xz.part-006+unpi5_PXP_ubuntu24_04.img.xz.part-007 unpi5_PXP_ubuntu24_04.img.xz
```

El orden es importante. Las partes deben unirse desde:

```text
part-001
```

hasta:

```text
part-007
```

Cuando el proceso termine, se creará el archivo:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

Verifica que el archivo final existe:

```bat
dir
```

El archivo reconstruido debería pesar aproximadamente **9 GB**, ya que corresponde a la unión de las 7 partes.

---

## 5) Grabar la imagen con Raspberry Pi Imager

No descomprimas manualmente el archivo `.img.xz`.

Aunque el archivo expandido puede ocupar alrededor de **64 GB**, no necesitas generar manualmente el `.img`.
**Raspberry Pi Imager puede grabar directamente el archivo comprimido `.img.xz`.**

El archivo que debes usar es:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

No selecciones las partes individuales.

---

### 5.1 Abrir Raspberry Pi Imager como administrador

1. Abre el menú de inicio.
2. Busca **Raspberry Pi Imager**.
3. Haz clic derecho sobre el programa.
4. Selecciona **Ejecutar como administrador**.

Esto es recomendable porque Raspberry Pi Imager necesita permisos para formatear y escribir sobre la microSD o SSD.

---

### 5.2 Seleccionar el modelo de Raspberry Pi

En Raspberry Pi Imager:

1. Selecciona el modelo de Raspberry Pi.
2. Para este backup, selecciona **Raspberry Pi 5**.

---

### 5.3 Seleccionar la imagen personalizada

En la sección del sistema operativo:

1. Baja hasta el final de la lista.
2. Selecciona **Usar personalizado** o **Use custom**.
3. Busca el archivo reconstruido:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

4. Selecciónalo.

---

### 5.4 Seleccionar la microSD o SSD

Selecciona la microSD o SSD donde quieres restaurar el sistema.

> **Advertencia:** este proceso borrará todos los datos del dispositivo seleccionado.

Se recomienda usar una microSD o SSD de al menos **64 GB**.

---

### 5.5 Escribir la imagen

Haz clic en **Siguiente** y luego en **Write / Escribir**.

Raspberry Pi Imager escribirá la imagen y después realizará una verificación.

Este proceso puede tardar bastante tiempo. En una microSD de 64 GB puede tardar alrededor de **2 horas** entre escritura y verificación, dependiendo de la velocidad del dispositivo y del lector usado.

---

## 6) Iniciar la Raspberry Pi 5

Cuando Raspberry Pi Imager termine:

1. Expulsa la microSD o SSD de forma segura.
2. Conéctala a la Raspberry Pi 5.
3. Enciende la Raspberry Pi.

Como esta es una imagen restaurada del sistema anterior, la configuración del sistema ya debería estar incluida.

Si el backup tenía configurado NoMachine, la Raspberry Pi debería poder aparecer nuevamente en NoMachine después de iniciar.

Si la Raspberry Pi no aparece inmediatamente:

1. Espera un momento mientras termina de iniciar.
2. Verifica que esté encendida.
3. Revisa que el ventilador se active.
4. Intenta conectarte nuevamente desde NoMachine.

En algunos casos puede ser necesario reiniciar la Raspberry Pi 5 usando el botón físico:

1. Mantén presionado el botón hasta que cambie el estado de la luz.
2. Vuelve a presionarlo para encenderla.
3. Espera a que el sistema inicie correctamente.

---

## 7) Problemas comunes

### Los archivos pesan pocos KB

Eso significa que probablemente descargaste solo los punteros de Git LFS.

Solución:

```bash
git lfs install
git lfs pull
```

Luego revisa de nuevo el tamaño de los archivos en `ubuntuImage`.

Cada parte debería pesar aproximadamente **1.3 GB**.

---

### `copy /b` no funciona

Asegúrate de estar usando **CMD como administrador**.

No ejecutes `copy /b` directamente en PowerShell.

El comando correcto debe ejecutarse en CMD:

```bat
copy /b /y unpi5_PXP_ubuntu24_04.img.xz.part-001+unpi5_PXP_ubuntu24_04.img.xz.part-002+unpi5_PXP_ubuntu24_04.img.xz.part-003+unpi5_PXP_ubuntu24_04.img.xz.part-004+unpi5_PXP_ubuntu24_04.img.xz.part-005+unpi5_PXP_ubuntu24_04.img.xz.part-006+unpi5_PXP_ubuntu24_04.img.xz.part-007 unpi5_PXP_ubuntu24_04.img.xz
```

---

### El archivo `.xz` queda dañado

Revisa lo siguiente:

* Debes tener exactamente **7 partes**.
* Las partes deben pesar aproximadamente **1.3 GB** cada una.
* Debes unirlas en orden desde `part-001` hasta `part-007`.
* No debes intentar abrir o descomprimir una parte individual.
* El archivo final debe llamarse:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

---

### Raspberry Pi Imager no muestra el archivo

Asegúrate de haber seleccionado:

```text
Use custom
```

o:

```text
Usar personalizado
```

El archivo que debes seleccionar es:

```text
unpi5_PXP_ubuntu24_04.img.xz
```

No selecciones las partes individuales.

---

### Raspberry Pi Imager pide formatear la microSD o SSD

Es normal. El proceso de escritura borrará el contenido del dispositivo.

Por eso es importante verificar que seleccionaste la microSD o SSD correcta antes de continuar.

---

## 8) Resumen rápido del proceso

```text
1. Abrir Git Bash.
2. Ejecutar git lfs install.
3. Clonar el repositorio.
4. Ejecutar git lfs pull.
5. Verificar que las 7 partes pesen aproximadamente 1.3 GB cada una.
6. Abrir CMD como administrador.
7. Entrar a la carpeta ubuntuImage.
8. Unir las partes con copy /b.
9. Abrir Raspberry Pi Imager como administrador.
10. Seleccionar Use custom / Usar personalizado.
11. Seleccionar unpi5_PXP_ubuntu24_04.img.xz.
12. Grabar la imagen en una microSD o SSD de al menos 64 GB.
13. Insertar la microSD o SSD en la Raspberry Pi 5 e iniciar el sistema.
```
