# ROS - Robot Operating System Dokumentace

Dokumentace je vytvářena pro ROS ve verzi Melodic (pro Ubuntu 18.04). Zároveň je doporučeno mít nainstalován ROS (konkrétně ros-melodic-full).

## Souborový systém

Souborový systém v ROSu se skládá ze dvou základních stavebních kamenů:

- **Packages** - Balíčky jsou základní organizační jednotky zdrojových kódů. Každý package může obsahovat knihovny, spustitelné soubory, skripty, nebo další věci..
- **Manifests** - Manifest popisuje package. Obsahuje závislosti mezi packagemi a zaznamenává informace o package jako jeho verzi, tvůrce, licenci, atd..



### Nástroje v souborovém systému

*Nástroje jsou určitě dostupné po nainstalování ros-melodic-desktop-full*!

#### *rospack*

Příkaz vypíše v terminálu informace o daném balíčku.

` $ rospack find [package_name] // Tento konkrétní příkaz vypíše, kde se zadaný package v souborovém systému nachází` 

`$ rospack find roscpp`

#### *roscd*

Umožňuje změnit pracovní adresář přímo do package

`$ roscd [location_name|package_name]`

`$ roscd roscpp`

`$ roscd log // Tento příkaz vás přesune do složky, kde ROS ukládá log soubory (pozn. musíte předtím už spustit nějaký program v ROSu, jinak to nebude fungovat)`

#### *rosls*

Umožňuje vypsat adresářovou strukturu ROS package

`$ rosls [location_name|package_name]` 

`$ rosls roscpp_tutorials`



### Vytváření ROS Package (balíčku)

ROS využívá k sestavování (buildění) balíčků *Catkin building system*.

**Co musí splňovat catkin Package?**

Package musí obsahovat:

-  ***package.xml*** - v souladu s catkinem
  - *package.xml poskytuje základní informace o package*

- ***CMakeLists.txt*** - vyžaduje taky catkin
  - *Popisuje, jak má cmake zbuildit (sestavit) package*
- Každý package musí mít svojí vlastní složku

#### Catkin workspace

Předtím než vytvoříme package musíme mít nainstalovaný caktin a vytvořit tzv. catkin workspace (*catkin by měl být nainstalovaný už po instalaci ros-melodic-desktop-full*)

##### Vytvoření catkin workspace 

```bash
$ mkdir -p ~/catkin_ws/src 
$ cd ~/catkin_ws/
$ catkin_make // Vytvoří nám složky build, devel, src
$ source devel/setup.bash // Source do bashe
$ echo $ROS_PACKAGE_PATH // Měla by se nám vypsat cesta k src složce
```

#### Vytváření package

Tento návod ukazuje práci se skriptem *catkin_create_pkg*, který vytvoří package za nás.

```bash
$ cd ~/catkin_ws/src // Musíme se přemístit do catkin workspace
$ catkin_create_pkg <nazev_package> [zavislost1] [zavislost2]
$ catkin_create_pkg beginner_tutorial std_msgs rospy roscpp
```

Poslení příkaz vytvoří složku *beginner_tutorial*, která bude obsahovat *package.xml* a *CMakeLists.txt*.

```
$ rospack depends1 beginner_tutorials
$ rospack depends beginner_tutorials
```

*První příkaz zobrazí závislosti našeho nově vytvořeného package (pouze, ale jen do první úrovně). Druhý příkaz zobrazí závislosti ve všech úrovních*.

#### Buildění package (sestavení package)

Příkazem *catkin_make* zbuildíte všechny package, které máte v *"~/caktin_ws/src"* složce.

```bash
$ cd ~/catkin_ws/
$ catkin_make
```





## ROS - struktura systému

ROS se dá rozdělit pomocí grafové struktury na:

- **Nodes** - "Uzly" - Node je spustitelný soubor, který využívá ROS ke komunikaci s ostatními nody (uzly)
- **Messages** - "Zpráva" - Datový typ, používaný při publikování nebo odebírání Topiců
- **Topics** - "Téma" - Nody mohou publikovat messages do Topiců, stejně tak mohou i odebírat (subscribnout) messages z topiců
- **Master** - "Kořenový uzel" - pomáhá Nodům se mezi sebou "najít"
- **rosout** - ekvivalent ROSu k stdout/stderr
- **roscore** - Master + rosout + parameter server

### Vyzkoušení si roscore, rosnode, rosrun

Pokud si chceme spustit roscore na lokálním počítači musíme si nejdřív nastavit proměnné v bash. Lepší, ale je si vložit příkazy do *~/.bashrc* souboru úplně nakonec a po té spustit příkaz `$ source ~/.bashrc` (to nám zajistí, že pokaždé, když spustíme konzoli, vytvoří a nastaví se nám proměnné).

```bash
$ export ROS_HOSTNAME=localhost
$ export ROS_MASTER_URI=http://localhost:11311
```

V případě, že se chceme připojit přímo na robota Breach, musíme vložit na úplný konec souboru *~/.bashrc* tyto hodnoty.

```bash
export ROS_MASTER_URI=http://192.168.1.14:11311
export ROS_HOSTNAME=192.168.1.100
```

#### Použití roscore

Spustí roscore proces.

```bash
$ roscore
```

#### Použití rosnode

*rosnode* je program příkazové řádky, který nám dokáže zobrazit informace o nodech. Otevřete si nové terminálové okno a v tom starém nechte spuštěný *roscore*.

```bash
$ rosnode list // Vypíše všechny aktuálně běžící nodes v ROSu
$ rosnode info /rosout // Vypíše info o daném node
$ rosnode -h // zobrazí všechny dostupné parametry k rosnode
```

#### Použití rosrun

*rosrun* je program příkazové řádky, který nám umožuje spustit package jako node.

```bash
$ rosrun [nazev_package] [nazev_node]
```

Na vyzkoušení si můžeme spustit *turtlesim* package.

```bash
$ rosrun turtlesim turtlesim_node
```

*Pozn. Pokud v dalším terminálovém okně spustíte příkaz "rosnode list" uvidíte, že tam přibyl node "/turtlesim"*

### ROS Topics

V novém terminálovém okně si spusťte tento node

```bash
$ rosrun turtlesim turtlesim_teleop_key
```

Nyní se ujistěte, že jste naklinutí v terminálovém okně a začněte mačkat na šipky. Uvidíte, že se želva začala po ploše pohybovat

![](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=turtle_key.png)

Nyní vidíte názorný příklad, jak spolu *turtlesim_node* a *turtle_teleop_key* nody komunikují přes **ROS Topic**.

*turtle_teleop_key* **publikuje** stisknuté klávesy do topicu, zatímco *turtlesim* **odebírá** ten samý topic, a tím získává informace o stisknutých klávesách.

 #### Použití rqt_graph

*rqt_graph* je package, který nám dokáže vytvořit graf s nodes a zobrazit jejich komunikaci skrze topics. Nejdříve je, ale potřeba ho nainstalovat.

```bash
$ sudo apt-get install ros-melodic-rqt
$ sudo apt-get install ros-melodic-rqt-common-plugins
```

Nyní ho můžeme spustit v dalším terminálovém oknu.

```bash
$ rosrun rqt_graph rqt_graph
```

V nově otevřeném okně vidíme naše dva nody (*/teleop_turtle* a */turtlesim*) a zároveň vidíme šipku od */teleop_turtle* k */turtlesim*. To nám právě značí, že */teleop_turtle* publikuje topic (v tomto případě se jmenuje */turtle1/comman_velocity*) a node */turtlesim* ho odebírá.

#### Použití rostopic

*rostopic* je nástroj příkazové řádky, který je schopen nám zobrazit informace o topics.

```bash
$ rostopic echo [nazev_topicu] // Bude na konzoli vypisovat všechny messages z daného topicu
$ rostopic list // zobrazí seznam všech aktivních topics
$ rostopic -h // zobrazí všechny dostupné parametry k rostopic
```

### ROS Messages

Komunikace pomocí topics se provadí s použitím messages, které se posílají mezi nody. K tomu aby si vydavatel (publisher) a odebírající (subsciber) node rozuměli, musí oba nody pracovat se stejným **typem** zpráv. To znamená, že typ topicu je dán typem zpráv, které se pomocí něj posílají. 

Typ zpráv, které se posílají skrze topic se dá zjistit pomocí příkazu

``` bash
$ rostopic type [nazev_topicu]
```

 Vyzkoušíme si to na našem příkladu s želvou

```bash
$ rostopic type /turtle1/cmd_vel
```

Vypíše se toto

```bash
geometry_msgs/Twist
```

Můžeme se podívat z čeho konkrétně se tento typ skládá, a to pomocí příkazu

```bash
$ rosmsg show geometry_msgs/Twist
```

Příkaz nám vráti toto

```bash
geometry_msgs/Vector3 linear
  float64 x
  float64 y
  float64 z
geometry_msgs/Vector3 angular
  float64 x
  float64 y
  float64 z
```

Nyní, když víme, co přesně za typ zprávy posílá node */teleop_turtle* do topicu */turtle1/cmd_vel* , můžeme si vyzkoušet poslat vlastní zprávu.

#### Použití rostopic pub

*rostopic pub* pošle zprávu do topicu. Použití

```bash
$ rostopic pub [nazev_topicu] [typ_zpravy] [argumenty]
```

Opět můžeme vyzkoušet na našem příkladu 

```bash
$ rostopic pub -1 /turtle1/cmd_vel/ geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
```

Můžeme pozorovat, že želva se začala pomalu otáčet do kruhu. Pojďme si postupně rozebrat všechny argumenty příkazu.

- **-1** => způsobí, že se daná zpráva pošle pouze jednou

- **/turtle1/cmd_vel/** => název topicu, do kterého chceme odeslat zprávu

- **geometry_msgs/Twist** => typ zprávy, kterou chceme poslat do topicu

- **--** => dvojitá pomlčka říká option parserovi že žádný z následujích argumentů není option

- **'[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'** => jak jsme si už ukázali, typ *geometry_msgs/Twist* obsahuje dva vektory skládající se ze tří floatů

### ROS Services

Services (služby) jsou další způsob, jak mohou mezi sebou nody komunikovat. Services umožňují nodům odesílat **requesty** (žádosti) a přijímat **responses** (odpovědi).

#### Použití rosservice

```bash
rosservice list         vypíše informace o aktivních services
rosservice call         zavolá service s určitými argumenty
rosservice type         vypíše typ service
rosservice find         najde services podle jejich typu
rosservice uri          vypíše ROSRPC uri topicu
```

Více informací na

http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams

### Použití rqt_console a roslaunch

*rqt_console* slouží k zobrazování výstupu z nodů. *rqt_logger_level* nám umožňuje nastavit úroveň "výřečnosti" nodů.

Spustíme si každý program v jednom terminálu

```bash
$ rosrun rqt_console rqt_console
$ rosrun rqt_logger_level rqt_logger_level
```

Když schválně pošleme tuto zprávu do topicu naší želvy, začne se želva pohybovat smwrem ke zdi až do ní narazí. *(pozn. samozřejmě nám musí běžet roscore a turtlesim)*

```bash
$ rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}'
```

V *rqt_console* nám začnou vyskakovat zprávy, kterými nás node upozorňuje, že želva narazila do zdi.

#### Použití roslaunch

*roslaunch* je program příkazové řádky který umí spouštět tzv. *launch soubory*. Launch soubory nám primárně slouží pro spuštění vícero nodů.

```bash
$ roslaunch [nazev_package] [nazevsouboru.launch]
```

Pojďme si napsat vlastní roslaunch soubor. Musíme mít vytvořený nějaký vlastní package, jak na to se dá zjistit v kapitole *Vytváření ROS Package (balíčku)*, kterou najdete v této dokumentaci.

Přesuneme se ke zdrojovým kódům našeho package

```bash
$ roscd beginner_tutorial
```

V ní vytvoříme složku *launch* a přesuneme se do ní

```bash
$ mkdir launch && cd launch
```

*Pozn. složku, ve které budeme ukládat naše launch soubory se nemusí nutně jmenovat "launch", nicméně je to dobrý zvyk. Příkaz roslaunch se podívá do našeho package a automaticky najde všechny .launch soubory*

Nyní vytvořme soubor *turtlemimic.launch* a vložte do něho tento kód

```xml
<launch>

    <!-- 
        Vytvoříme dvě skupiny s namespace tagem turtlesim1 a turtlesim2.
        Toto nám zajistí spustit dva želví simulátory bez toho aniž by došlo
        k nějakým konfliktům. 
    -->
    <group ns="turtlesim1">
        <node pkg="turtlesim" name="sim" type="turtlesim_node" />
    </group>

    <group ns="turtlesim2">
        <node pkg="turtlesim" name="sim" type="turtlesim_node" />
    </group>

    <!--
        Vytvoříme tzv. mimic node s input a output topicy přejmenovanými na turtlesim1 a turtlesim2.
        Toto přejmenování nám zajístí, že turtlesim2 nám bude kopírovat turtlesim1.
    --> 
    <node pkg="turtlesim" name="mimic" type="mimic">
        <remap from="input" to="turtlesim1/turtle1" />
        <remap from="output" to="turtlesim2/turtle1" />
    </node>

</launch>
```

Tento launch soubor nám vytvoří dva simulátory želvy a zajistí, že 2. želva bude opakovat všechny pohyby, které provádí 1. želva

Nyní nám stačí spustit náš *turtlemimic.launch* soubor

```bash
$ roslaunch beginner_tutorial turtlemimic.launch
```

Spustí se nám dvě okna s želvami. Nyní nám stačí poslat zprávu do topicu první želvy.

```bash
$ rostopic pub /turtlesim1/turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'
```

Obě želvy by se měli opakovaně točit dokola.

### Vytvoření ROS *msg* a *srv*

- **msg** - jsou jednoduché textové soubory, které popisují jednotlivé položky v ROS zprávách (messages). Tyto soubory jsou používány pro generování zdrojových kódů zpráv.
- **srv** - soubory, které popisují určitou service. Jsou složené ze dvou částí: request a response

*msg* soubory jsou uloženy ve složce *msg* určitého package, a *srv* soubory zase ve složce *srv* 

Zde jsou příklady *msg* souboru a *srv* souboru

```
Header header
string child_frame_id
geometry_msgs/PoseWithCovariance pose
geometry_msgs/TwistWithCovariance twist
```

```
int64 A
int64 B
---
int64 Sum
```

#### Použití msg

Opět použijeme náš již vytvořený package *beginner_tutorial*

```bash
$ roscd beginner_tutorial
$ mkdir msg
$ echo "int64 num" > msg/Num.msg
```

Pokračování => http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Using_msg

