# 🔒 SOC Lab : pfSense + Suricata IDS + Wazuh Active Response

[![GitHub stars](https://img.shields.io/github/stars/ArmandDonjang-lab/SOC-Lab-pfSense-Suricata-Wazuh)](https://github.com/ArmandDonjang-lab/SOC-Lab-pfSense-Suricata-Wazuh)
[![License MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

**SOC Lab production-ready** : Détection & blocage automatique des attaques réseau (scan Nmap, brute-force SSH).

**Auteur** : Armand Donjang - Douala, CM | [LinkedIn](https://linkedin.com/in/armand-donjang)

## 🎯 Features

- ✅ **Suricata IDS pur** → `eve.json` → syslog-ng → Wazuh
- ✅ **Règles custom brute-force** : `sid:10001xx`
- ✅ **Active Response Wazuh** : `timeout="600"` (10min)
- ✅ **Zéro agent Wazuh** : logs centralisés pfSense
- ✅ **Tests réels** : Kali → Ubuntu victime

## 🏗️ Architecture
Kali Linux ───🛡️─── pfSense(Suricata IDS) ─── syslog-ng ─── Wazuh All-in-One
│ │
└──────────[scan nmap][brute-force ssh]──────┘
│
Ubuntu Server (victime)

text

## 🚀 Quick Deploy (15min)

### 1. pfSense → Suricata IDS
Packages → Suricata → [LAN Interface]
☑️ IDS mode only (PAS IPS)
☑️ EVE.json output → syslog-ng

text

### 2. Wazuh Active Response
```xml
/var/ossec/etc/ossec.conf :
<active-response>
  <command>host-deny</command>
  <location>localhost</location>
  <rules_id>1960012,1000100</rules_id>
  <timeout>600</timeout>
</active-response>
```

### 3. Test d'attaque
```bash
# Kali
nmap -sS 192.168.1.100
hydra -l root -P rockyou.txt ssh://192.168.1.100

# Résultat → IP Kali bloquée 10min
```

## 📊 Résultats Mesurés

| Attaque                   | Détection | Blocage  |
|---------------------------|-----------|----------|
| Nmap scan                 | 0.8s      |    /     |
| SSH Brute-force (5 fails) | 12s       | ✅ 10min |

## 📂 Contenu du Repo
configs/
├── suricata-custom.rules # Vos règles brute-force
├── wazuh-active-response.xml # Config AR
└── syslog-ng.conf # pfSense → Wazuh
docs/
├── architecture.drawio # Schéma réseau
└── deployment.md # Guide étape par étape

## 📄 License

MIT - Utilisez librement pour vos labs !

---

**⭐ Star si utile !** Questions → [Issues](https://github.com/ArmandDonjang-lab/SOC-Lab-pfSense-Suricata-Wazuh/issues)
