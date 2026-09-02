# Test Set per Steampunk Spotter (XLAB Partnership)

Questo pacchetto contiene una serie di playbook Ansible e il relativo file `requirements.yml` pensati per testare la piattaforma **Steampunk Spotter** ([spotter.steampunk.si](https://spotter.steampunk.si/)).

## Struttura del Pacchetto

- `requirements.yml`: Contiene collezioni con versioni datate per far emergere vulnerabilità note (CVE) e avvisi di deprecazione.
- `playbooks/`:
  1. `01_base_hostname_banner.yml`: Utilizzo del modulo legacy `ios_config` anziché dei moduli dichiarativi dedicati.
  2. `02_interfaces_legacy.yml`: Moduli di interfaccia e switchport sconsigliati/obsoleti.
  3. `03_vlans_declarative.yml`: Approccio dichiarativo per VLAN (verifica di opzioni e stati idonei).
  4. `04_routing_ospf.yml`: Configurazione OSPF/Static Routes e validazione degli schemi di dizionario.
  5. `05_acls_security.yml`: Gestione utenti e ACL con evidenza di vulnerabilità e credenziali in chiaro.
  6. `06_multivendor_logging.yml`: Gestione multi-vendor (Cisco IOS & Arista EOS).

## Come Utilizzarlo

1. **Via Web Interface**: Carica i file su [spotter.steampunk.si](https://spotter.steampunk.si/) assicurandoti di includere anche `requirements.yml`.
2. **Via CLI**:
   ```bash
   pip install steampunk-spotter
   spotter scan playbooks/ requirements.yml
   ```
