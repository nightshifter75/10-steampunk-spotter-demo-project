# Playbook di test per Steampunk Spotter

Set di 10 playbook Ansible per apparati **Cisco IOS** (switch/router), pensati
per essere caricati su spotter.steampunk.si e testarne la funzione di **Scan**:
rilevamento CVE, moduli deprecati/obsoleti, e utilizzo scorretto dei moduli.

## Mappa dei playbook

| # | File | Cosa testa |
|---|------|------------|
| 1 | 01_interfaces_config.yml | Uso di `ios_config` (comandi raw) al posto del modulo dedicato `ios_interfaces`/`ios_l2_interfaces` → **wrong module usage** |
| 2 | 02_vlans_legacy.yml | Modulo `ios_vlan` **deprecato** (va usato `ios_vlans`) → **obsolescenza** |
| 3 | 03_l3_interfaces_legacy.yml | Modulo `ios_l3_interface` **deprecato** (va usato `ios_l3_interfaces`) → **obsolescenza** |
| 4 | 04_static_routes_legacy.yml | Modulo `ios_static_route` **deprecato** (va usato `ios_static_routes`) → **obsolescenza** |
| 5 | 05_acl_config.yml | Modulo moderno `ios_acls` → esempio **corretto** di confronto |
| 6 | 06_ntp_logging.yml | NTP con modulo corretto `ios_ntp_global` + logging con `ios_command` invece del dedicato `ios_logging_global` → **wrong module usage** |
| 7 | 07_bgp_config.yml | Modulo moderno `ios_bgp_global` → esempio **corretto** |
| 8 | 08_user_accounts.yml | Modulo `ios_user` **deprecato** + password in chiaro nel playbook → **obsolescenza + security hygiene** |
| 9 | 09_facts_backup.yml | `ios_facts` con `gather_subset: all` (troppo ampio) → possibile **suggerimento di ottimizzazione** |
| 10 | 10_full_declarative_modern.yml | Stessa configurazione VLAN/L2/L3 dei playbook 2-3 ma con **soli resource module moderni** → termine di paragone "best practice" |

## File di supporto

- `requirements.yml`: versioni delle collection **volutamente datate**
  (cisco.ios 3.3.0, ansible.netcommon 5.1.0) per verificare se Spotter
  segnala CVE note o versioni obsolete a livello di collection.
- `inventory.ini`: inventario di esempio (IP fittizi) da usare se Spotter
  richiede un inventory per il collegamento fra scan e host target.

## Come usarli nella demo

1. Carica i singoli file `.yml` (o l'intera cartella, se Spotter supporta
   l'upload multiplo/di un repo) nella sezione **Scan** della piattaforma.
2. Aspettati che i playbook 2, 3, 4, 8 producano warning di **deprecazione/obsolescenza**.
3. Aspettati che i playbook 1 e 6 producano segnalazioni di **best practice /
   wrong module usage** (uso di `ios_config`/`ios_command` al posto dei moduli dedicati).
4. Aspettati che il playbook 8 segnali anche il problema della **password in chiaro**.
5. Usa i playbook 5, 7, 10 come controprova "pulita" per mostrare come
   dovrebbe apparire un risultato di scan senza warning rilevanti.
6. Se richiesto, mostra anche `requirements.yml` per il controllo CVE a
   livello di versione delle collection.

> Nota: gli IP, gli hostname e le credenziali sono fittizi e servono solo a
> scopo dimostrativo per l'analisi statica; questi playbook non sono pensati
> per essere eseguiti realmente contro apparati di produzione.
