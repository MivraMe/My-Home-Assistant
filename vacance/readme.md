# Mode vacance / Gestion absence

### Le mode vacance est composé de différent capteurs utiles dans certaines automatisation notament ceux pour le chauffage.

#### *Chaque personnes doit avoir un inbut boolean (helper) qui seras utilisé pour les modes vacances*
___
## Le capteur présence
Le capteur de présence est activé si au moins une personne est à la maison
La valeur du capteur varie de `False` à `True`
```
sensor:
  - platform: template
    sensors:
      vacance_presence_sensor:
        unique_id: vacance_presence_sensor
        friendly_name: "Vacance présence"
        value_template: >-
          {{ is_state('input_vacance_human1', 'off') or
             is_state('input_boolean.vacance_human2', 'off') or
             is_state('input_boolean.vacance_human3', 'off') or
             is_state('input_boolean.vacance_human4', 'off') }}
```

## Le capteur absence
Le capteur d'absence est activé si au moins une personne n'est pas à la maison
La valeur du capteur varie de `False` à `True`
```
sensor:
  - platform: template
    sensors:
      vacance_absence_sensor:
        unique_id: vacance_absence_sensor
        friendly_name: "Vacance absence"
        value_template: >-- platform: template
    sensors:
      absence_parents_sensor:
        unique_id: vacance_absence_parent_sensor
        friendly_name: "Absence parents"
        value_template: "{{ is_state('input_boolean.vacance_', 'on') and is_state('input_boolean.vacance_johann', 'on') }}"
          {{ is_state('vacance_human4', 'on') or
             is_state('input_boolean.vacance_human1', 'on') or
             is_state('input_boolean.vacance_human2', 'on') or
             is_state('input_boolean.vacance_human3', 'on') }}
```

## Le capteur de 2 personnes
Le capteur d'absence est activé si les deux personnes sont en vacance

Le capteur est utile pour la chambre d'un couple

La valeur du capteur varie de `False` à `True`
```
- platform: template
    sensors:
      absence_parents_sensor:
        unique_id: vacance_absence_parent_sensor
        friendly_name: "Absence parents"
        value_template: "{{ is_state('input_boolean.vacance_human1', 'on') and is_state('input_boolean.vacance_human2', 'on') }}"
```

# NON TESTÉ
## Le capteur 2+
Le capteur d'absence est activé si au moins deux personnes ne sont pas à la maison

Il est utile dans des automatisation de notification, ou de déclenchement prioritaire

Contrairement aux autres, le capteur utilise la localisation des appareils

La valeur du capteur varie de `Off` à `On`
```
sensor:
- platform: template
    sensors:
      personnes_2_plus_absentes_sensor:
        unique_id: personnes_2_plus_absentes_sensor
        friendly_name: "Personnes 2+ Absentes"
        value_template: >-
          {% set personnes_presentes = [states('device_tracker.device1'), states('device_tracker.device2'), states('device_tracker.device3'), states('device_tracker.device4')] %}
          {% set personnes_absentes = personnes_presentes | selectattr('state','eq','not_home') | list %}
          {{ 'on' if personnes_absentes | length >= 2 else 'off' }}
```
