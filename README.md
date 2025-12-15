# UNPi5_Image_Ubuntu24_04

Este repositorio almacena un archivo grande dividido en **7 partes** .

Las partes suelen llamarse así (recomendado):

- `unpi5_PXP_ubuntu24_04.img.xz.part-001`
- `unpi5_PXP_ubuntu24_04.img.xz.part-002`
- …
- `unpi5_PXP_ubuntu24_04.img.xz.part-007`

> Importante: **cada parte por separado NO es un `.xz` válido**. Primero debes unirlas para recuperar el archivo original.

---

## 0) El repo usa Git LFS (recomendado)

Antes de unir, asegúrate de haber descargado los archivos reales de LFS (no “punteros” pequeños):

### Ubuntu 24.04
```bash
git lfs install
git lfs pull
```

### Windows 11
```powershell
git lfs install
git lfs pull
```

Luego verifica que las partes pesan cientos de MB/GB:

- Ubuntu:
  ```bash
  ls -lh unpi5_PXP_ubuntu24_04.img.xz.part-*
  ```
- Windows (PowerShell):
  ```powershell
  dir unpi5_PXP_ubuntu24_04.img.xz.part-*
  ```

Si ves archivos de pocos KB, LFS no descargó el contenido: ejecuta `git lfs pull` de nuevo.

---

## 1) Unir las partes en Ubuntu 24.04 LTS

En la carpeta donde estén las partes, ejecuta:

```bash
cat unpi5_PXP_ubuntu24_04.img.xz.part-* > unpi5_PXP_ubuntu24_04.img.xz
```

### Verificación rápida
- Probar integridad del `.xz`:
  ```bash
  xz -t unpi5_PXP_ubuntu24_04.img.xz
  ```

### (Opcional) Verificar con SHA256
Si en el repo existe un archivo de checksum (por ejemplo `unpi5_PXP_ubuntu24_04.img.xz.sha256`), valida así:

```bash
sha256sum -c unpi5_PXP_ubuntu24_04.img.xz.sha256
```

Si no existe, puedes crear uno (una sola vez, después de reconstruir correctamente):

```bash
sha256sum unpi5_PXP_ubuntu24_04.img.xz > unpi5_PXP_ubuntu24_04.img.xz.sha256
```

---

## 2) Unir las partes en Windows 11

### Opción A: PowerShell (recomendado)
Este método es robusto y no depende de trucos del shell.

1) Abre **PowerShell** en la carpeta que contiene las partes.
2) Ejecuta:

```powershell
$OutFile = "unpi5_PXP_ubuntu24_04.img.xz"
$PartFiles = 1..7 | ForEach-Object { "{0}.part-{1:000}" -f $OutFile, $_ }

$out = [System.IO.File]::Create($OutFile)
try {
  foreach ($p in $PartFiles) {
    $inp = [System.IO.File]::OpenRead($p)
    try { $inp.CopyTo($out) } finally { $inp.Close() }
  }
} finally {
  $out.Close()
}
```

#### Verificar
- Probar integridad del `.xz` (si tienes 7-Zip o xz instalado, mejor). Si no, al menos calcula el hash:

```powershell
Get-FileHash "unpi5_PXP_ubuntu24_04.img.xz" -Algorithm SHA256
```

Si tienes un `.sha256` en el repo:

```powershell
# Muestra el hash del archivo reconstruido para compararlo con el .sha256
Get-FileHash "unpi5_PXP_ubuntu24_04.img.xz" -Algorithm SHA256
```

### Opción B: CMD (Command Prompt)
Si prefieres CMD:

```bat
copy /b unpi5_PXP_ubuntu24_04.img.xz.part-001+unpi5_PXP_ubuntu24_04.img.xz.part-002+unpi5_PXP_ubuntu24_04.img.xz.part-003+unpi5_PXP_ubuntu24_04.img.xz.part-004+unpi5_PXP_ubuntu24_04.img.xz.part-005+unpi5_PXP_ubuntu24_04.img.xz.part-006+unpi5_PXP_ubuntu24_04.img.xz.part-007 unpi5_PXP_ubuntu24_04.img.xz
```

> Nota: en CMD es importante escribirlas **en orden**.

---

## 3) Problemas comunes

### “Me da error al unir / el `.xz` queda corrupto”
- Confirma que tienes **exactamente 7 partes** y que sus tamaños son razonables.
- Asegúrate de que las partes estén **en orden** (`001`…`007`).
- Si usas Git LFS, ejecuta:
  - `git lfs pull`
  - y revisa tamaños (si son KB, no tienes el contenido real).

### “Mis partes se llaman `part-aa`, `part-ab`, ...”
Eso pasa si se usó `split` sin sufijos numéricos. En Ubuntu puede funcionar igual:

```bash
cat unpi5_PXP_ubuntu24_04.img.xz.part-* > unpi5_PXP_ubuntu24_04.img.xz
```

En Windows, en ese caso, es mejor renombrar a `001..007` o ajustar la lista `$PartFiles` para que coincida con tus nombres.

---

## 4) Después de unir

Ya con `unpi5_PXP_ubuntu24_04.img.xz` reconstruido, puedes descomprimir:

- Ubuntu:
  ```bash
  unxz -k unpi5_PXP_ubuntu24_04.img.xz
  ```

(El uso final depende de tu flujo: por ejemplo, grabar la imagen en una SD/SSD con tu herramienta preferida.)

