# Helper

## AC/DC Charging

<details>
<summary>secc/controller/simulator.py</summary>

```python
async def get_supported_energy_transfer_modes():
    ac_three_phase = EnergyTransferModeEnum.AC_THREE_PHASE_CORE
    return [ac_three_phase]
```
</details>

## Charge Schedules

<details>
<summary>secc/controller/simulator.py</summary>

```python
async def get_sa_schedule_list():
    sa_schedule_list: List[SAScheduleTuple] = []
    p_max_1 = PVPMax(multiplier=0, value=SimEVSEController._pmax_value, unit=UnitSymbol.WATT)
    p_max_schedule_entry_1 = PMaxScheduleEntry(
        p_max=p_max_1, time_interval=RelativeTimeInterval(start=0, duration=5)
    )
    p_max_schedule = PMaxSchedule(
        schedule_entries=[p_max_schedule_entry_1]
    )
    sa_schedule_tuple = SAScheduleTuple(
        sa_schedule_tuple_id=1,
        p_max_schedule=p_max_schedule,
        sales_tariff=None,
    )
    sa_schedule_list.append(sa_schedule_tuple)
    return sa_schedule_list
```
</details>

# Tests

## 1. Leistung im laufenden Ladevorgang verändern

<details>
<summary>Tests</summary>

```commandline
echo 8000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test1.log
```

### 1. 11kw

```commandline
echo 4000 > pmax.txt
```

#### Resultat:
>
### 2. 5kw

```commandline
echo 5000 > pmax.txt
```

#### Resultat:
>
### 3. 22kw

```commandline
echo 22000 > pmax.txt
```

#### Resultat:
>

</details>

## 2. Leistung 0 = Ladestopp? Was ist minimal?

<details>
<summary>Tests</summary>

### 1.1 0kw

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test2.1.1.log
```

```commandline
echo 0 > pmax.txt
```

#### Resultat:
>

### 1.2 0kw - simulator.py: test = "2-end-with-0"

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test2.1.2.log
```

```commandline
echo 0 > pmax.txt
```

##### Resultat:
>

### 1.3 0kw - simulator.py: test = "2-start-with-0"

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test2.1.3.log
```

```commandline
echo 0 > pmax.txt
```

##### Resultat:
>

### 2. Minumum ausloten

```commandline
echo ... > pmax.txt
```

#### Resultat:
>

</details>
 
## 3. ISO15118 Kommunikation bei PWM >5% möglich?
<details>
<summary>Tests</summary>

### 1. Kommunikation initialisierbar?

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test3.1.log
```

#### Resultat:
>

### 2. PWM nach Aufbau der Kommunikation erhöhen

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test3.2.log
```

#### Resultat:
>

</details>

## 4. Experimentieren mit ChargeSchedule duration

<details>
<summary>Tests</summary>

### 1. Nur einen Schedule mit kurzer duration verwenden. Was passiert nach Ablauf dieser? - simulator.py: test = "4.1"

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test4.1.log
```

#### Resultat:
>

### 2. Schedules staffeln, verändert sich der Ladestrom entsprechend der durations? - simulator.py: test = "4.2"

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test4.2.log
```

#### Resultat:
>

</details>


## 5. SOC vorhanden?

<details>
<summary>Tests</summary>

### 1. AC Laden

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test5.1.log
```

#### Resultat:
>

### 2. ISO15118 DC charging bei echtem AC charging um an SOC zu kommen?

```commandline
echo 4000 > pmax.txt
~/iso15118/venv/bin/iso15118 2>&1 | tee ~/testlogs/test5.2.log
```

#### Resultat:
>

</details>