# Manual breve — Instalación de Linux + Configuración de Redes
Guía concisa basada en dos videos: **Instalación de Linux Mint Xfce en VirtualBox** y **Configuración de redes (Windows 11 & Linux Mint)**.

## Enlaces a los videos
- Video 1 — Instalación de Linux Mint Xfce en VirtualBox: (https://youtu.be/r6_uLFrN_Xo)
- Video 2 — Configuración de Redes (Windows 11 & Linux Mint):(https://youtu.be/SGbPDgSB26E)

---

## 1) Instalación de Linux Mint Xfce en VirtualBox (sin eliminar Windows)

### Requisitos
- Windows 11 + Oracle VM VirtualBox
- ISO Linux Mint Xfce (reciente)

### Pasos
1. Descargar:
   - VirtualBox: https://www.virtualbox.org  
   - ISO Mint (Xfce): https://linuxmint.com (Download)
2. VirtualBox → **Nueva** → Nombre: `Linux Mint Xfce`  
   Tipo: *Linux* · Versión: *Ubuntu (64-bit)* · RAM: **2–4 GB** · Disco **VDI dinámico ≥20 GB**
3. Configuración → **Almacenamiento** → Unidad óptica → *Elegir archivo de disco* (ISO de Mint)
4. Iniciar VM → **Start Linux Mint** → Escritorio en vivo → **Install Linux Mint**
5. Asistente: Idioma/Teclado → (opcional) códecs → Tipo: **Borrar disco e instalar** *(solo el disco virtual)* → Zona horaria → Usuario/Contraseña
6. Reiniciar → retirar ISO si lo pide → Arranca Mint → Conectar a red → **Gestor de actualizaciones**

### Verificación
```bash
hostnamectl
ip addr
ping -c 3 8.8.8.8
Si falla
No aparece Ubuntu (64-bit): habilitar VT-x/AMD-V en BIOS/UEFI

Sin red: cambiar NAT ↔ Bridged; instalar Guest Additions

2) Configuración de Redes
2.1 Cableadas (Ethernet)
A) Windows 11 ↔ Windows 11 (IP estática)
Conectar por cable (directo o vía switch).

Configuración → Red e Internet → Ethernet → Propiedades → Editar IP → Manual (IPv4)

PC-A: 192.168.10.10 / 255.255.255.0

PC-B: 192.168.10.11 / 255.255.255.0

Si no hay ping: Firewall → Reglas de entrada → habilitar Echo Request (ICMPv4)

cmd
Copy
Edit
ping 192.168.10.11  :: desde PC-A
ping 192.168.10.10  :: desde PC-B
B) Linux Mint ↔ Linux Mint
Conexiones de Red → Cableada → Editar → IPv4 Manual

Eq-1: 192.168.20.10/24

Eq-2: 192.168.20.11/24

Reiniciar conexión y probar:

bash
Copy
Edit
ping -c 3 192.168.20.11
ping -c 3 192.168.20.10
C) Windows 11 ↔ Linux Mint
Windows IPv4 Manual: 192.168.30.10/24

Linux IPv4 Manual: 192.168.30.11/24

Habilitar ICMP en Windows si falla

cmd
Copy
Edit
ping 192.168.30.11   :: Windows
bash
Copy
Edit
ping -c 3 192.168.30.10   # Linux
2.2 Inalámbricas (Wi-Fi)
A) Windows 11 ↔ Windows 11
Ambos al mismo SSID (preferible misma banda).

Ver IP (DHCP) y probar:

cmd
Copy
Edit
ipconfig /all
ping 192.168.1.51   :: desde PC-A (PC-B = 192.168.1.51)
ping 192.168.1.50   :: desde PC-B (PC-A = 192.168.1.50)
B) Linux Mint ↔ Linux Mint
Ambos al mismo SSID (WPA2/WPA3).

(Opc.) Hotspot en un equipo y conectar el otro.

Probar:

bash
Copy
Edit
ip addr
ping -c 3 <IP_del_otro_equipo>
C) Windows 11 ↔ Linux Mint
Ambos al mismo SSID (evitar “AP Isolation/Guest”).

Ver IP y probar:

c
Copy
Edit
ipconfig /all
ping 192.168.1.61   :: Windows (Linux = 192.168.1.61)
bash
Copy
Edit
ip addr
ping -c 3 192.168.1.60   # Linux (Windows = 192.168.1.60)
3) Diagnóstico rápido
Checklist
Enlace/SSID correcto → IP y máscara en misma subred

En Windows: permitir ICMP en Firewall

Cambiar cable/puerto/switch o canal Wi-Fi si hay interferencias

Comandos útiles
cmd
Copy
Edit
:: Windows
ipconfig /all
ping <IP>
tracert <IP_o_dominio>
bash
Copy
Edit
# Linux
ip addr
ping -c 3 <IP>
traceroute <IP_o_dominio>
nmcli dev status
Créditos
Autores: Wagner Jacob Ajcúc Sambrano · Jefferson Saúl Ajcúc Sambrano
Semestre: Segundo Semestre 2025 · USAC — Facultad de Ingeniería — Escuela de Ingeniería en Ciencias y Sistemas
