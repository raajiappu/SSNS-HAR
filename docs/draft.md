## Smart Sensor Network Systems | Human Activity Recognition
s



-->trash
A-Обновить и Добавить в Видео, пока на скрытом слое
### ChangeLog: {App | Text | Video}
  `Plan`
00. +++ Run HIRO in the Amazon cloud, Linux Machine
  { Cy.Tunnel }
01. +++ Change Data Types for Graph BD Nodes, Using Ontology ::CustomApplicationData --> `ogit/*`
  +++Test LunchApp Functionality
02. +++ Change `Engine-CLI` --> Official `hiro-CLI`
  { Use both for debugging | slightly different functionality }
  ?Jira branch to add functionality
03. +++ Change `WorkFlow`, First Create Issue, then Write KIs
04. +++ Add Task/Issue/KI `GetMenu`, `ExtractMenu`, `ParseMenu`, `SaveMenu`::
  ++SaveMenuChoice_Num, SaveCurrentMenu_Str
  ++Write/Read Data to TS
05. --- Use UI.KAT to create KI
06. --- Add Advanced Profile :: 'Fish Preferences'
07. --- Use 'VacationProfile', if outlook calendar shows vacation
08. +++ Use TimeSeries to Store and Analyze Statistical Data
  ?Example to visualize Statistical Data
09. --- Calendar Module to Track {Date, Time}
10. --- Implement Basic Learning Mechanism
  ?{ Victor | Stefan | Ludmilla }
11. --- Fill DB with data automatically, Use `HIRO`::issue/ki
12. --- App+Text+Video

#### Remove Warning
[root@ip-10-4-8-129]#
```
export LANGUAGE=en_US:en
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

Б-найти подходящий раздел
### Additional!!!
```
hiro ki undeploy
hiro ki list | xargs hiro ki undeploy
hiro issue get --history <`issue_id`>
hiro ki delete --real-delete <`ki_id`>
hiro ki list | xargs hiro ki delete --real-delete
hiro issue list | xargs engine resolve issue
hiro ki list | xargs hiro ki get
```

### Instance :: Run HIRO in the Amazon Cloud, Linux Machine

//Installed by DevOps.@Arseny
```
ssh root@ec2-34-245-72-134.eu-west-1.compute.amazonaws.com
```

```
amerdeev@mbpx-amerdeev-5 ~> ssh-add
Enter passphrase for /Users/amerdeev/.ssh/id_rsa:
Identity added: /Users/amerdeev/.ssh/id_rsa (/Users/amerdeev/.ssh/id_rsa)
```

#### Kill the Old One
```
[root@ip-10-4-0-98 ~]# poweroff
Connection to ec2-34-245-173-201.eu-west-1.compute.amazonaws.com closed by remote host.
Connection to ec2-34-245-173-201.eu-west-1.compute.amazonaws.com closed.
```

#### Check System Status

```
/opt/autopilot/admin/status-autopilot.sh
```

```
[root@ip-10-4-7-233 ~]# /opt/autopilot/admin/status-autopilot.sh
IUR  zookeeper - Zookeeper
IUR  kafka - Kafka
IUR  cassandra - Cassandra
IUR  elasticsearch - Elasticsearch
IUR  wso2is - WSO2 Identity Server
IUR  validator - HIRO KI Validator
IUR  graphit - GraphIT Server
IUR  engine - HIRO Engine

Components checked   : 8
Components installed : 8
Components running   : 8
Components working   : 8
```

#### Check Engine Status

```
engine status
```

```
[root@ip-10-4-7-233 ~]# engine status
|name                  |error                         |meta
|----------------------|------------------------------|-------------------------
|check                 |                              |{"license":true,"coord...
|cluster               |                              |{"state":"connected"}
|graphit.auth          |                              |{"connected":true}
|graphit.ki_auth       |                              |{"connected":true}
|graphit.questions     |                              |{"connected":true}
|graphit.updater       |                              |{"connected":true}
|graphit.userinput     |                              |{"connected":true}
|graphit.userinput_auth|                              |{"connected":true}
|license               |                              |{"valid_to":"2019-01-0...
|topology              |                              |{"state":"done","resou...
```

#### Create ssh Tunnel from the Amazon Cloud to Internal Company Service:

//Done by Engine.@Cy
https://lunch.arago.de/choice.cgi

//Only Cy can help
Test
```
curl -k --data "USER=Andrey+Merdeev&LANGUAGE=English" -X POST https://lunch.arago.de:10443/choice.cgi
```

### Data Modeling: Ontology = Appropriate Data Types and Relations

https://github.com/arago/OGIT

Ontology description:
  https://arago.github.io/OGIT/group__httpwwwpurlorgogitSoftwareApplication.html
  https://graphit.co/ogit/list.php?dataset=ontology#obj-ogit-Software-Application

`Big picture` representation of all data [2D, 3D, search mechanism]
//ToDo.Try.

#### Extend Global GraphIT Ontology
New Entity: `ogit/UserMeta/Preferences`
New Relation: `ogit/Person` {likes} `ogit/UserMeta/Preferences`

//ToDo.Done Update Ontology, pull request

//ToDo {?likes --> uses--connects}
//ToDo {likes.?Weight=[0,1]}, or `rating approach from ogit/Forum`
//ToDo {KI find all connections with {`like`} and chose max `weight`}

//ToDo.Done statical scheme with `ogit/DataType` for LunchApp

#### Updating the Local GraphIT Ontology

Approach_A
ToDo.Done Extend `Ontology`:: `ogit:Person {likes} ogit:UserMeta.Preferences`

Approach_B
//ToDo Extend `Ontology` Facebook/Yandex/Google like style, `User Preferences`
//Later

>>
Once OGIT master branch is updated, the changes are automatically built and pushed every hour to
https://graphit.co/schemas/graphit-ontology.yaml

To update ogit ontology on local GraphIT installation, please run:
```
cd /opt/autopilot/conf/graphit-server/ontology
wget https://graphit.co/schemas/graphit-ontology.yaml -O graphit-ontology.yaml
```

and run script
```
cd /opt/autopilot/setup/helpers
./load_ontology /opt/autopilot/conf/graphit-server/ontology/graphit-ontology.yaml
```

then to ensure that all changes are correctly loaded you may restart GraphIT:
```
/opt/autopilot/admin/start-autopilot.sh -r graphit
```

Hiro-engine
```
/opt/autopilot/admin/start-autopilot.sh -r engine
```

or
```
systemctl restart graphit-server
systemctl restart hiro-engine
```

and Check
```
/opt/autopilot/admin/status-autopilot.sh
engine status
```

#### Add [Ontology] Variables `ToDo`

1) One can add `ToDo` variables using `REST` interface
or

2) ?Machinery

Define Variables
```
cd /opt/autopilot/conf/graphit-server/ontology
vi variables.yaml
```

Load Variables
```
cd /opt/autopilot/setup/helpers
./load_variables
```

Restart Graphit
```
/opt/autopilot/admin/start-autopilot.sh -r graphit
```

Define `ToDo` variables
`InitializeYear`

```
- {ogit/Automation/todo: true, ogit/_creator: amerdeev, ogit/accessControl: public,
  ogit/description: InitializeYear, ogit/name: InitializeYear,
  ogit/subType: issue}
```

Define variables to `Store` data
`CurrentYear`

```
- {ogit/Automation/todo: false, ogit/_creator: amerdeev, ogit/accessControl: public,
  ogit/description: CurrentYear, ogit/name: CurrentYear,
  ogit/subType: issue}
```

#### Data Types for LunchApp

```
Node_User_AndreyMerdeev.json
{
  "ogit/_type": "ogit/Person",
  "ogit/email": "amerdeevT@arago.de",
  "ogit/firstName": "Andrey",
  "ogit/lastName": "Merdeev",
  "ogit/_reader": "arago.co"
}
```

```
Node_User_DmitryRuss.json
{
  "ogit/_type": "ogit/Person",
  "ogit/email": "drussT@arago.de",
  "ogit/firstName": "Dmitry",
  "ogit/lastName": "Russ",
  "ogit/_reader": "arago.co"
}
```

```
Node_MenuPreferencesProfile_Vegetarian.json
{
  "ogit/_type": "ogit/UserMeta/Preferences",
  "/LunchProfileChoice": "2",
  "ogit/_reader": "arago.co"
}
```

```
Node_MenuPreferencesProfile_AlwaysThirdChoice.json
{
  "ogit/_type": "ogit/UserMeta/Preferences",
  "/LunchProfileChoice": "3",
  "ogit/_reader": "arago.co"
}
```

```
Node_LunchApp.json
{
  "ogit/_type": "ogit/Software/Application",
  "ogit/name": "LunchApp",
  "ogit/url": "https://lunch.arago.de:10443/choice.cgi",
  "ogit/_reader": "arago.co"
}
```

##### Node.Timeseries_UserName, Type: `ogit/Timeseries`

```
Node_Timeseries_UserName
{
  type:
    ogit/Timeseries
  attributes:
    optional:
      ogit/description
      ogit/instance
      ogit/name
      ogit/unit
  incoming edges:
    ogit/tracks <= ogit/Person
}
```

```
Node_Timeseries_DmitryRuss.json
{
  "ogit/_type": "ogit/Timeseries",
  "ogit/description": "Save Timeseries Data for the LunchApp",
  "ogit/_reader": "arago.co"
}
```

### Routing

Conf
```
vim /opt/arago/conf/hiro_engine.conf
```

#### Define Nodes and Edges Types Consider in Routing

```
# Node types to consider in routing
routing.nodetypes = "ogit/MARS/Machine", "ogit/MARS/Software", "ogit/MARS/Resource", "ogit/MARS/Application", "ogit/Person", "ogit/Software/Application", "ogit/UserMeta/Preferences", "ogit/Timeseries"

# Edge types to consider in routing
routing.edgetypes = "ogit/dependsOn", "ogit/uses", "ogit/likes", "ogit/tracks", "ogit/generates"
```

After Adding Change, Engine has to be restarted
```
/opt/autopilot/admin/start-autopilot.sh -r engine
```

В-Обновить, добавить в видео, найти место в тексте и видео
### A Short Start
```
ssh root@ec2-34-245-72-134.eu-west-1.compute.amazonaws.com
root@ec2-34-245-72-134.eu-west-1.compute.amazonaws.com

https://ec2-18-202-243-7.eu-west-1.compute.amazonaws.com:8443/_static/explorer/index.html

engine status
/opt/autopilot/admin/status-autopilot.sh

User::
export HIRO_TOKEN=$(hiro_get_token -U hirouser -P pw4user1 --batch-mode | sed s/export\ HIRO_TOKEN=//g)
Admin::
export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)

User::
hiro_get_token -U testusera -P testusera
hiro_get_token -U hirouser -P pw4user1
Admin::
hiro_get_token -U hiroadmin -P pw4hiroadmin
```

### Customize HIRO-CLI

Сервис авторизации, аутентификации?
Добавить кусочек в видео?
Web server WSO2
```
https://ec2-34-245-72-134.eu-west-1.compute.amazonaws.com:9443/carbon/user/user-mgt.jsp?pageNumber=0&
```

### Authorization

```
hiro_get_token -U testusera -P testusera
```

User
```
export HIRO_TOKEN=$(hiro_get_token -U hirouser -P pw4user1 --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```

Admin
```
export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```

```
hiro --help
```

Check privileges
```
curl -X GET 0:8888/_me?_TOKEN=$HIRO_TOKEN
```

```
curl -X GET 0:8888/_me?_TOKEN=$HIRO_TOKEN | python -m json.tool
```

#### Search.Show All Nodes

```
hiro vertex search "ogit/_id:*"
```
but that will give you only all you have access to,
could take some hours on global graph

```
[root@ip-10-4-8-129 ~]# hiro vertex search "ogit/_id:*" > AllNodes_hirouser
1523 Items found
```

```
//~Approximate Stats `USER`
ogit/_type:
  ogit/OntologyAttribute: 453
  ogit/OntologyEntity:    349
  ogit/OntologyVerb:      277
  ogit/Auth/Application:  5
  ogit/Auth/Account:      1
  ogit/Auth/Role:         1

Total:                    1086
ogit/_created-on:         1086
```

```
[root@ip-10-4-8-129 ~]# hiro vertex search "ogit/_id:*" >AllNodes_hiroadmin
12016 Items found
```
### Creating DataStructure Using HIRO-CLI
{Furtherer it will be done automatically by `Connector` ?Subsystem of HIRO}

#### Create Node_User_UserName

`ogit/Person`
```
hiro vertex create -t ogit/Person Node_User_AndreyMerdeev.json
hiro vertex create -t ogit/Person Node_User_DmitryRuss.json
hiro vertex create -t ogit/Person Node_User_StefanPitz.json
```

```
Node_User_AndreyMerdeev.json -> amerdeevT@arago.de
Node_User_DmitryRuss.json -> drussT@arago.de
```

```
[root@ip-10-4-1-97 KI]# hiro vertex update --help
usage: hiro [<general options>] vertex update  [<specific options>]
            [<update JSON files>]>
Options
 -f,--file <fileName>   file that contains update JSON (requires -i/--id)
 -h,--help              show usage
 -i,--id <vertexId>     vertex ID to update (requires -f/--file)
    --threads <arg>     number of parallel executions (only for file mode)
```

```
hiro vertex update -i amerdeevT@arago.de -f Node_User_AndreyMerdeev.json
hiro vertex update -i drussT@arago.de -f Node_User_DmitryRuss.json
```

Check attributes
```
hiro vertex get amerdeevT@arago.de
hiro vertex get drussT@arago.de
```

```
[root@ip-10-4-1-97 ~]# hiro vertex get amerdeevT@arago.de
{
  "/UserName": "Andrey+Merdeev",
  "ogit/_created-on": "1539172452836",
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "amerdeevT@arago.de",
  "ogit/_is-deleted": "false",
  "ogit/_modified-by": "testusera",
  "ogit/_modified-by-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_modified-on": "1539701460170",
  "ogit/_owner": "testusera",
  "ogit/_reader": "arago.co",
  "ogit/_type": "ogit/Person",
  "ogit/_v": "5",
  "ogit/_v-id": "1539701460170-UOeBS9",
  "ogit/_xid": "amerdeevT@arago.de",
  "ogit/email": "amerdeevT@arago.de",
  "ogit/firstName": "Andrey",
  "ogit/lastName": "Merdeev"
}
1 Item found
```

```
[root@ip-10-4-1-97 ~]# hiro vertex get drussT@arago.de
{
  "/UserName": "Dmitry+Russ",
  "ogit/_created-on": "1539172468462",
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "drussT@arago.de",
  "ogit/_is-deleted": "false",
  "ogit/_modified-by": "testusera",
  "ogit/_modified-by-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_modified-on": "1539701468697",
  "ogit/_owner": "testusera",
  "ogit/_reader": "arago.co",
  "ogit/_type": "ogit/Person",
  "ogit/_v": "3",
  "ogit/_v-id": "1539701468697-j6wHbo",
  "ogit/_xid": "drussT@arago.de",
  "ogit/email": "drussT@arago.de",
  "ogit/firstName": "Dmitry",
  "ogit/lastName": "Russ"
}
1 Item found
```

#### Create Node_LunchApp

In a directory `run/data/` create a file
Node_LunchApp.json
```
{
  "ogit/name": "LunchApp",
  "ogit/url": "https://lunch.arago.de:10443/choice.cgi",
  "ogit/_reader": "arago.co"
}
```
//or `"ogit/_reader": "public"`

And create Node in DB-Graphit using hiro-CLI command:
```
hiro vertex create -t ogit/Software/Application Node_LunchApp.json
```

which will give an `<id_node>` back,
so we will use this `<id_node>` further to create links between nodes

Output `<id_node>` is cjpe0fpx10yo27g0209wj9d6b
`Node_LunchApp.json -> cjpe0fpx10yo27g0209wj9d6b`

```
hiro vertex update -i cjpe0fpx10yo27g0209wj9d6b -f Node_LunchApp.json
```

Check attributes
```
hiro vertex get cjpe0fpx10yo27g0209wj9d6b
```

```
[root@ip-10-4-1-97 ~]# hiro vertex get cjpe0fpx10yo27g0209wj9d6b

{
  "ogit/_created-on": "1539173659007",
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/_is-deleted": "false",
  "ogit/_modified-by": "testusera",
  "ogit/_modified-by-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_modified-on": "1539701591440",
  "ogit/_owner": "testusera",
  "ogit/_reader": "arago.co",
  "ogit/_type": "ogit/Software/Application",
  "ogit/_v": "4",
  "ogit/_v-id": "1539701591440-uYzmrq",
  "ogit/name": "LunchApp",
  "ogit/url": "https://lunch.arago.de/choice.cgi"
}
1 Item found
```

#### Search for the Node
```
hiro vertex search "ogit/_id:amerdeevT@arago.de"
hiro vertex search "ogit/name:LunchApp"
hiro vertex search "ogit/_type:ogit/Person"
```

#### Create Node_MenuPreferencesProfile_Choice

Node_MenuPreferencesProfile_Vegetarian.json
```
{
  "/LunchProfileChoice": "2",
  "ogit/_reader": "arago.co"
}
```

```
hiro vertex create -t ogit/UserMeta/Preferences Node_MenuPreferencesProfile_AlwaysFirstChoice.json
hiro vertex create -t ogit/UserMeta/Preferences Node_MenuPreferencesProfile_AlwaysThirdChoice.json
```

```
Node_MenuPreferencesProfile_Vegetarian.json -> cjnnan5nz01lfur46z6v1ey3z
Node_MenuPreferencesProfile_AlwaysThirdChoice.json -> cjpe7l4ha16z67g02l4g88vdr
```

```
hiro vertex update -i cjnnan5nz01lfur46z6v1ey3z -f Node_MenuPreferencesProfile_Vegetarian.json
hiro vertex update -i cjpe7l4ha16z67g02l4g88vdr -f Node_MenuPreferencesProfile_AlwaysThirdChoice.json
```

```
hiro vertex search "ogit/_type:ogit/UserMeta/Preferences"
```

```
[root@ip-10-4-7-233 data]# hiro vertex search "ogit/_type:ogit/UserMeta/Preferences"
{
  "/LunchProfileChoice": "3",
  "ogit/_created-on": 1540393754721,
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "cjpe7l4ha16z67g02l4g88vdr",
  "ogit/_is-deleted": false,
  "ogit/_modified-by": "testusera",
  "ogit/_modified-by-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_modified-on": 1540393754721,
  "ogit/_owner": "testusera",
  "ogit/_reader": "arago.co",
  "ogit/_type": "ogit/UserMeta/Preferences",
  "ogit/_v": 1,
  "ogit/_v-id": "1540393754721-cETf6Q"
}
{
  "/LunchProfileChoice": "2",
  "ogit/_created-on": 1540393710191,
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "cjnnan5nz01lfur46z6v1ey3z",
  "ogit/_is-deleted": false,
  "ogit/_modified-by": "testusera",
  "ogit/_modified-by-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_modified-on": 1540393710191,
  "ogit/_owner": "testusera",
  "ogit/_reader": "arago.co",
  "ogit/_type": "ogit/UserMeta/Preferences",
  "ogit/_v": 1,
  "ogit/_v-id": "1540393710191-hBX1MU"
}
2 Items found
```

#### Create Node_Timeseries_UserName

`Node_Timeseries_DmitryRuss.json -> cjoq0d4mdaqvtur46tgsvet0q`
Node_Timeseries_DmitryRuss.json
```
{
  "ogit/_type": "ogit/Timeseries",
  "ogit/description": "Save Timeseries Data for the LunchApp",
  "ogit/_reader": "arago.co"
}
```

```
hiro vertex create -t ogit/Timeseries Node_Timeseries_DmitryRuss.json
```

`Node_Timeseries_AndreyMerdeev.json -> cjoq07aqwaniour46eaj9g1zp`
Node_Timeseries_AndreyMerdeev.json
```
{
  "ogit/_type": "ogit/Timeseries",
  "ogit/description": "Save Timeseries Data for the LunchApp",
  "ogit/_reader": "arago.co"
}
```

-->{CS}
```
hiro vertex create -t ogit/Timeseries Node_Timeseries_AndreyMerdeev.json
```

//ToDo{#id.0003}

##### Delete Node

```
hiro vertex delete <id>
```

-->{amerdeev!!!}
```
[root@hirodocker automation_data]# hiro vertex delete cjj8thi1w0i0u6w22v03u5d5w
vertex cjj8thi1w0i0u6w22v03u5d5w successfully deleted
```

Once a vertex is deleted, graphit will automatically delete all edges associated to that vertex.
The `graph DB` takes care of its consistency.

### Create Links Between Nodes: Logic and Structure

```
usage: hiro [<general options>] edge create  [<specific options>]
            <from/out-vertex> <edge-type> <to/in-vertex>
```
Node_User_AndreyMerdeev --> Node_LunchApp
hiro edge create "amerdeevT@arago.de" "ogit/uses" "cjpe0fpx10yo27g0209wj9d6b"
Edge created: amerdeevT@arago.de -ogit/uses-> cjpe0fpx10yo27g0209wj9d6b
hiro vertex search
```
hiro vertex search "ogit/_id:*ogit/uses*"
```
Node_User_DmitryRuss --> Node_LunchApp
hiro edge create "drussT@arago.de" "ogit/uses" "cjpe0fpx10yo27g0209wj9d6b"
Node_User_StefanPitz --> Node_LunchApp
hiro edge create "spitzT@arago.de" "ogit/uses" "cjpe0fpx10yo27g0209wj9d6b"


Node_User_AndreyMerdeev --> Node_MenuPreferencesProfile_AlwaysThirdChoice
  hiro edge create "amerdeevT@arago.de" "ogit/likes" "cjpe7l4ha16z67g02l4g88vdr"
Node_User_DmitryRuss --> Node_MenuPreferencesProfile_AlwaysThirdChoice
  hiro edge create "drussT@arago.de" "ogit/likes" "cjpe7l4ha16z67g02l4g88vdr"
Node_User_StefanPitz --> Node_MenuPreferencesProfile_AlwaysFirstChoice
  hiro edge create "spitzT@arago.de" "ogit/likes" "cjpe952hj17xi7g020uraywtw"

#### Node_User_AndreyMerdeev --> Node_LunchApp
```
hiro edge create "amerdeevT@arago.de" "ogit/uses" "cjpe0fpx10yo27g0209wj9d6b"
```
```
Edge created: amerdeevT@arago.de -ogit/uses-> cjpe0fpx10yo27g0209wj9d6b
```

#### Node_User_DmitryRuss--> Node_LunchApp
```
hiro edge create "drussT@arago.de" "ogit/uses" "cjpe0fpx10yo27g0209wj9d6b"
```
```
Edge created: drussT@arago.de -ogit/uses-> cjpe0fpx10yo27g0209wj9d6b
```

#### Node_User_AndreyMerdeev --> Node_Timeseries_AndreyMerdeev
```
hiro edge create "amerdeevT@arago.de" "ogit/tracks" "cjoq07aqwaniour46eaj9g1zp"
Edge created: amerdeevT@arago.de -ogit/tracks-> cjoq07aqwaniour46eaj9g1zp
```

#### Node_User_DmitryRuss--> Node_Timeseries_DmitryRuss
```
hiro edge create "drussT@arago.de" "ogit/tracks" "cjoq0d4mdaqvtur46tgsvet0q"
Edge created: drussT@arago.de -ogit/tracks-> cjoq0d4mdaqvtur46tgsvet0q
```

#### Node_User_AndreyMerdeev --> Node_MenuPreferencesProfile_AlwaysThirdChoice
```
hiro edge create "amerdeevT@arago.de" "ogit/likes" "cjpe7l4ha16z67g02l4g88vdr"
```

#### Node_User_DmitryRuss -->  Node_MenuPreferencesProfile_AlwaysThirdChoice
```
hiro edge create "drussT@arago.de" "ogit/likes" "cjpe7l4ha16z67g02l4g88vdr"
```

##### Delete Link
```
usage: hiro [<general options>] edge delete  [<specific options>]
            <from/out-vertex> <edge-type> <to/in-vertex>
```

//?Show Edge
//?Show All Data Structure


-->||

### Execution
### Solve Task === Create Issues, Deploy KIs {`decision making` and `action making`}

#### Debugging

Engine log
`/var/log/arago/hiro-engine/hiro-engine.log`

Engine audit
```
engine audit -d <issue_id> <ki_id>
```

-->
Task_01__SetMenu_User_ForAll
#### Create Issues

Issue__SetMenu_User_ForAll.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__SetMenu_User_ForAll",
  "/SetMenuForAll": "true"
}
```

#### Create KIs

KI__SetMenu_User_ForAll.ki
```
ki
  id: "KI__SetMenu_User_ForAll"
on
  ogit/_id
  ogit/firstName
  ogit/lastName
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
    ogit/url
  end
when
  SetMenuForAll
do
  LOCAL::UserName = "${ogit/firstName}+${ogit/lastName}"
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SetMenu = "true", SelectMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserName} with SetMenu and SelectMenu")
```

```
KI__SetMenu_User_ForAll.ki -> created with id=cjnnayx3t02ksur46ibt4kgzz
```

KI__SelectMenu_ByUserPreferences.ki
```
ki
  id: "KI__SelectMenu_ByUserPreferences"
on
  ogit/_id
  with ogit/UserMeta/Preferences@out:ogit/likes
    LunchProfileChoice
  end
when
  UserName
  SelectMenu
do
  add(LunchSelected["Mon"] = LunchProfileChoice)
  add(LunchSelected["Tue"] = LunchProfileChoice)
  add(LunchSelected["Wed"] = LunchProfileChoice)
  add(LunchSelected["Thu"] = LunchProfileChoice)
  add(LunchSelected["Fri"] = LunchProfileChoice)
  delete(SelectMenu)
```

```
KI__SelectMenu_ByUserPreferences.ki -> created with id=cjnnb786u03bzur466ab104bf
```

KI__SetMenu_User_UserName.ki
```
ki
  id: "KI__SetMenu_User_UserName"
on
  ogit/_id
  ogit/lastName
  ogit/firstName
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
    ogit/url
  end
when
  UserName
  SetMenu
  list LunchSelected
  LunchSelected["Mon"] as Mon
  LunchSelected["Tue"] as Tue
  LunchSelected["Wed"] as Wed
  LunchSelected["Thu"] as Thu
  LunchSelected["Fri"] as Fri
do
  code: code, body: body = action("HTTP", method: "POST", url: ogit/url, body: "LANGUAGE=Englisch&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Mon}&tuesday=${Tue}&wednesday=${Wed}&thursday=${Thu}&friday=${Fri}", insecure: "true")
  log("${ogit/url} => ${body}")
  delete(LunchSelected)
  delete(UserName)
  delete(SetMenu)
```

```
KI__SetMenu_User_UserName.ki -> created with id=cjnnb8gll03jvur46f2xhg200
```

-->


KI__ResolveOnIdleForAll.ki
```
ki
  id: "KI__ResolveOnIdleForAll"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  ExecIdle > 60
  SetMenuForAll
do
  delete(SetMenuForAll)
```
KI__ResolveOnIdleForAll.ki -> created with id=cjnlw6hpv0wr7sb62j3o8lejx


## Execution

посты три

### Task_04__SetMenu_User_ForAll__WithGremlinQueryToGraph

#### Additional
```
hiro issue list | xargs engine resolve issue
```

#### Authorization
User
```
export HIRO_TOKEN=$(hiro_get_token -U hirouser -P pw4user1 --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```
Admin
```
export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```

#### Add Ontology Variables {`ToDo` and `Storage`}
```
cd /opt/autopilot/conf/graphit-server/ontology
```

`variables.yaml`

##### Task_SetMenu


#### Create Issues
Issue__SetMenu_User_ForAll__WithGremlinQueryToGraph.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__SetMenu_User_ForAll__WithGremlinQueryToGraph",
  "/SetMenuForAll": "true",
  "/CreateListOfUsers": "true"
}
```

Issue__GetMenu_User_ForAll.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__SetMenu_User_ForAll__WithGremlinQueryToGraph",
  "/SetMenuForAll": "true",
  "/CreateListOfUsers": "true"
}
```

```
//  "/A" : "1",
//   "=/List": ["foo", "bar"],
//  "=/ListWithKeys": [{"value": "foo", "key": "bar"}]
```

```
engine list issue
hiro issue list
hiro issue get --history cjo5ttpffwz62ur46jfyckkqq
hiro issue get --show-list-meta cjo5ttpffwz62ur46jfyckkqq
```

#### Create KIs
KI_id `cjnxmk8vcyfywur46pd2x9u1u`
KI__CreateListOfUsers.ki
```
ki
  id: "KI__CreateListOfUsers"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  CreateListOfUsers
do
  output: LOCAL::Output = query("gremlin",
    'inE("ogit/uses").outV();',
    root: "cjpe0fpx10yo27g0209wj9d6b",
    each: begin
      add(ISSUE::UserID = ogit/_id)
      add(ISSUE::UserName[ogit/_id] = "${ogit/firstName}+${ogit/lastName}")
    end)
  log(Output)
  delete(CreateListOfUsers)
```

KI_id `cjnxnvb3xylheur46kckowclv`
KI__CreateIssue_User_ListOfUsers.ki
```
ki
  id: "KI__CreateIssue_User_UserList"
on
  ogit/_id
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
  end
when
  UserID == ogit/_id
  UserName[ogit/_id]
do
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SaveHistory = "true", SelectMenu = "true", SetMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserID} with ${UserName} and ToDo--Var:: `SelectMenu` and `SetMenu`")
  add(ISSUE::CreateListOfIssues = "true")
  delete(UserID)
```

//Specify diff `KN`
`KI cjo5qy0fiwk65ur46rbka4uev deployed to KnowledgePool cjnn6stz000eat34609h2bcng`
KI_id `cjo5qy0fiwk65ur46rbka4uev`
KI__ResolveParentIssue.ki
```
ki
  id: "KI__ResolveParentIssue"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  not CreateListOfUsers
  not UserID
  SetMenuForAll
do
  delete(SetMenuForAll)
```

```
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
  end
```

KI_id `cjo5rc6h2wmhgur46f67os4p2`
KI__SelectMenu_ByUserPreferences.ki
```
ki
  id: "KI__SelectMenu_By_UserPreferences"
on
  ogit/_id
  with ogit/UserMeta/Preferences@out:ogit/likes
    LunchProfileChoice
  end
when
  UserName
  SelectMenu
do
  add(LunchSelected["Mon"] = LunchProfileChoice)
  add(LunchSelected["Tue"] = LunchProfileChoice)
  add(LunchSelected["Wed"] = LunchProfileChoice)
  add(LunchSelected["Thu"] = LunchProfileChoice)
  add(LunchSelected["Fri"] = LunchProfileChoice)
  delete(SelectMenu)
```

KI_id `cjo5rc6h2wmhgur46f67os4p2`
KI__SetMenu_User_UserName.ki
```
ki
  id: "KI__Set-Menu_User_UserName"
on
  ogit/_id
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
    ogit/url
  end
when
  UserName
  SetMenu
  list LunchSelected
  LunchSelected["Mon"] as Mon
  LunchSelected["Tue"] as Tue
  LunchSelected["Wed"] as Wed
  LunchSelected["Thu"] as Thu
  LunchSelected["Fri"] as Fri
do
  code: LOCAL::code, body: LOCAL::body = action("HTTP", method: "POST", url: ogit/url, body: "LANGUAGE=Englisch&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Mon}&tuesday=${Tue}&wednesday=${Wed}&thursday=${Thu}&friday=${Fri}", insecure: "true")
  log("${ogit/url} => ${body}")
  delete(LunchSelected)
  delete(UserName)
  delete(SetMenu)
```

-->{!!!}
## Execution
### Task_SetMenu
issue ki variables
### Task_SaveMenu

Useful tips
text+video

#### Authorization
User
```
export HIRO_TOKEN=$(hiro_get_token -U hirouser -P pw4user1 --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```
Admin
```
export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)
```

export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)
hiro issue list | xargs engine resolve issue

export HIRO_TOKEN=$(hiro_get_token -U hirouser -P pw4user1 --batch-mode | sed s/export\ HIRO_TOKEN=//g)
hiro issue put Issue__Initialize_ListOfUsers.json

export HIRO_TOKEN=$(hiro_get_token -U hiroadmin -P pw4hiroadmin --batch-mode | sed s/export\ HIRO_TOKEN=//g)
hiro issue list

hiro issue get cjou4mut603qg9846hjlydyy8

hiro issue get --show-list-meta cjo7dghs048odur466ac1jy67


##### Task_SaveMenu


##### Task_ReadTS
Add `ToDo` variables
  `ReadTSAll`
```
- {ogit/accessControl: public, ogit/name: ReadTSAll, ogit/subType: issue,
  ogit/description: ReadTSAll, ogit/Automation/todo: true}
- {ogit/accessControl: public, ogit/name: DebugFlag, ogit/subType: issue,
  ogit/description: DebugFlag, ogit/Automation/todo: true}
```

##### Task_

#### Create Issues
```
hiro issue list
engine list issue
hiro issue list | xargs engine resolve issue
hiro issue get --history cjo7dghs048odur466ac1jy67
hiro issue get --show-list-meta cjo7dghs048odur466ac1jy67
engine audit <issue_id> <ki_id>
```

#### Create KIs
```
hiro ki list
engine list ki
hiro ki list | xargs hiro ki undeploy
```

-->{!!!}
MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW1=
curl GET

MODE=choice&LANGUAGE=English&USER=${UserName_2}&KW=1

MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW2=
```
curl -k --data "MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW=2" https://lunch.arago.de:10443/choice.cgi
```



```
curl -k --data "MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW=1" https://lunch.arago.de:10443/choice.cgi
```

body: "MODE=choice&LANGUAGE=English&USER=${UserName}&KW=1"

MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW=1
MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW1=

```
curl -k --data "MODE=choice&LANGUAGE=English&USER=Andrey+Merdeev&KW=1" https://lunch.arago.de/choice.cgi
```

```
  {
    "ogit/_created-on" : "Wed, 24 Oct 2018 13:07:53 GMT"
    "ogit/_creator" : "graphit.co"
    "ogit/_graphtype" : "vertex"
    "ogit/_id" : "ogit/Automation/rankingType"
    "ogit/_is-deleted" : "false"
    "ogit/_modified-on" : "Wed, 24 Oct 2018 14:47:19 GMT"
    "ogit/_owner" : "graphit.co"
    "ogit/_tags" : "ogit,Automation"
    "ogit/_type" : "ogit/OntologyAttribute"
    "ogit/_v" : "4"
    "ogit/_v-id" : "1540392439841-KoYDEL"
    "ogit/_version" : "2.19.1.219"
    "ogit/description" : "Contains the type of KI ranking algorithm. "
    "ogit/ontologyAdminContact" : ""
    "ogit/ontologyCreator" : "cy@arago.de"
    "ogit/ontologyDeleter" : ""
    "ogit/ontologyModified" : ""
    "ogit/ontologyName" : "rankingType"
    "ogit/ontologyTechContact" : ""
    "ogit/ontologyType" : "attribute"
    "ogit/ontologyValidFrom" : "Tue May 01 00:00:00 UTC 2018"
    "ogit/ontologyValidUntil" : ""
    "ogit/ontologyValidationParameter" : "Simple,DecisionService,Random,Precalculated"
    "ogit/ontologyValidationType" : "fixed"
  }
```


```
ki
  id: "KI__CreateIssue_User_UserList"
on
  ogit/_id
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
  end
when
  not CreateListOfUsers
  UserID == ogit/_id
  UserName[ogit/_id]
  SetMenuForAll
do
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SetMenu = "true", SelectMenu = "true")
  log("created issue ${ID} for user ${UserID} with ToDo--Var:: `SetMenu` and `SelectMenu`")
  delete(UserID)
```

//`UserList == ogit/_id`
//UserName = "${ogit/firstName}+${ogit/lastName}"


KI__CreateListOfUsers_Uses_LunchApp.ki
```
ki
  id: "KI__CreateListOfUsers_Uses_LunchApp"
on # NODE::
  ogit/_id
  ogit/name == "LunchApp"
when # ISSUE::
  ListOfUsersNotCreated
do # ISSUE::
  output: LOCAL::Output = query("gremlin",
    'inE("ogit/uses").outV();',
    root: "cjpe0fpx10yo27g0209wj9d6b",
    each: begin # AnotherScope
      add(ISSUE::User = ogit/_id)
      add(ISSUE::UserName[ogit/_id] = "${ogit/firstName}+${ogit/lastName}")
    end)
  log(Output)
  delete(ListOfUsersNotCreated)
```

KI__CreateIssue_User_ListOfUsers.ki
```
ki
  id: "KI__CreateIssue_User_ListOfUsers"
on
  ogit/_id
  # ogit/name == "LunchApp"
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
  end
when
  User == ogit/_id
  # User
  UserName[User]
  SetMenuForAll
do
  # issueid: LOCAL::ID = create_issue(NodeID = User, SetMenu = "true", SelectMenu = "true", UserName = UserName)

  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SetMenu = "true", SelectMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserName} with SetMenu and SelectMenu")
  delete(ListOfUsers)
```


### TestA
#### Create Issues
Issue__CreateListOfUsers.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__CreateListOfUsers_Uses_LunchApp",
  "/ListOfUsersNotCreated": "true"
}
```

#### Create KIs
KI__CreateListOfUsers_Uses_LunchApp.ki
```
ki
  id: "KI__CreateListOfUsers_Uses_LunchApp"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  ListOfUsersNotCreated
do
  output: LOCAL::Output = query("gremlin",
    'inE("ogit/uses").outV();',
    root: "cjpe0fpx10yo27g0209wj9d6b",
    each: begin
      add(ISSUE::ListOfUsers["${ogit/firstName}+${ogit/lastName}"] = ogit/_id)
    end)
  log(Output)
  delete(ListOfUsersNotCreated)
```

KI__CreateIssue_User_ListOfUsers.ki
```
ki
  id: "KI__CreateIssue_User_ListOfUsers"
on
  ogit/_id
  ogit/firstName
  ogit/lastName
  with ogit/Software/Application@out:ogit/uses
    ogit/name == "LunchApp"
    ogit/url
  end
when
  ListOfUsers == ogit/_id
  SetMenuForAll
do
  LOCAL::UserName = "${ogit/firstName}+${ogit/lastName}"
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SetMenu = "true", SelectMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserName} with SetMenu and SelectMenu")
  delete(ListOfUsers)
```

KI__ResolveParentIssue.ki
```
ki
  id: "KI__ResolveParentIssue"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  not ListOfUsers
  SetMenuForAll
do
  delete(SetMenuForAll)
```


  list ListOfUsers

```
hiro issue get --show-list-meta cjnxf1nh9xl7eur468vwixm1v
```

```
cjnxbq03nx46fur46gxahb4m5
```

```
add(ListOfUsers["${ogit/firstName}+${ogit/lastName}"] = ogit/_id)

add(LunchSelected["Mon"] = LunchProfileChoice)
output: LOCAL::Output = query("gremlin", 'inE("ogit/uses").outV();', root: "cjpe0fpx10yo27g0209wj9d6b", each: begin add(User = ogit/_id) end)
```


KI__CreateListOfUsers_Uses_LunchApp.ki
```
ki
  id: "KI__CreateListOfUsers_Uses_LunchApp"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  ListOfUsersNotCreated
  list ListOfUsers
do
  output: LOCAL::Output = query("gremlin", 'inE("ogit/uses").outV();', root: "cjpe0fpx10yo27g0209wj9d6b", fields: "ogit/_id")
  log(Output)
  delete(ListOfUsersNotCreated)
```


```
hiro graph query -root $ROOT_VERTEX -Q 'outE("ogit/_owns").inV().has("ogit/_type", "ogit/Automation/KnowledgeItem");'

hiro graph query -root $ROOT_VERTEX -Q 'outE("ogit/_owns").inV().has("ogit/_type", "ogit/Automation/KnowledgeItem");'
```

```
hiro graph query -root cjpe0fpx10yo27g0209wj9d6b -Q 'inE("ogit/uses").outV();'
```

```
hiro graph query -root cjnnactjp00kqur46jqmhcoj9 -Q 'outE("ogit/_owns").inV();'
```



#### System Preparation

```
hiro ki undeploy
hiro ki list | xargs hiro ki undeploy
```

```
[root@ip-10-4-7-233 ~]# hiro ki list | xargs hiro ki undeploy
3 KIs found
KI cjnnb786u03bzur466ab104bf undeployed from KnowledgePool cjnn6stz000eat34609h2bcng
KI cjnnb8gll03jvur46f2xhg200 undeployed from KnowledgePool cjnn6stz000eat34609h2bcng
KI cjnnayx3t02ksur46ibt4kgzz undeployed from KnowledgePool cjnn6stz000eat34609h2bcng
```

```
[root@ip-10-4-7-233 ~]# hiro ki list
cjnnb786u03bzur466ab104bf
cjnnb8gll03jvur46f2xhg200
cjnnayx3t02ksur46ibt4kgzz
3 KIs found
[root@ip-10-4-7-233 ~]# engine list ki
[root@ip-10-4-7-233 ~]#
```

#### TestA
#### Create Issues
Issue__CreateListOfUsers.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__CreateListOfUsers_Uses_LunchApp",
  "/ListOfUsersNotCreated": "true"
}
```

#### Create KIs
KI__CreateListOfUsers_Uses_LunchApp.ki
```
ki
  id: "KI__CreateListOfUsers_Uses_LunchApp"
on
  ogit/_id
  ogit/name == "LunchApp"
when
  ListOfUsersNotCreated
do
  output: LOCAL::Output = query("gremlin", 'inE("ogit/uses").outV();', root: "cjpe0fpx10yo27g0209wj9d6b")
  log(Output)
```

```
KI__SetMenu_User_ForAll.ki -> created with id=cjnnayx3t02ksur46ibt4kgzz
```

#### Nodes and Edges Types Consider in Routing
```
# Node types to consider in routing
routing.nodetypes = "ogit/MARS/Machine", "ogit/MARS/Software", "ogit/MARS/Resource", "ogit/MARS/Application", "ogit/Person", "ogit/Software/Application", "ogit/UserMeta/Preferences", "ogit/Timeseries"

# Edge types to consider in routing
routing.edgetypes = "ogit/dependsOn", "ogit/uses", "ogit/likes", "ogit/tracks"
```
```
[root@ip-10-4-0-98 run]# vim /opt/arago/conf/hiro_engine.conf
```

[root@ip-10-4-0-98 run]# hiro ki put KI__SetMenu_User_UserName.ki
KI__SetMenu_User_UserName.ki -> created with id=cjnn597ww0z84lg627gtczsv9
[root@ip-10-4-0-98 run]# hiro ki put KI__SetMenu_User_ForAll.ki
KI__SetMenu_User_ForAll.ki -> created with id=cjnn59f8p0z8jlg62saa5amwm
[root@ip-10-4-0-98 run]# hiro ki put KI__SelectMenu_ByUserPreferences.ki
KI__SelectMenu_ByUserPreferences.ki -> created with id=cjnn59lci0z8vlg628m1jiwv4

!!!Issue.Resize
```
vi /opt/arago/conf/hiro_engine.conf
false
?wait
# Whether to put an issue into WAITING state when undefined variables are detected.
variable.wait_on_undefined = true
```
variable.wait_on_undefined = false

Restart engine
```
/opt/autopilot/admin/start-autopilot.sh -r engine
```

-->Task_05__GetMenu_NextWeek
#### Create Issues

Issue__GetMenu_NextWeek.json
```
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.Issue__GetMenu_NextWeek",
  "/GetMenu": "true"
}
```

#### Create KIs

KI__GetMenu_NextWeek.ki
```
ki
  id: "KI__GetMenu_NextWeek"
on
  ogit/_id
  ogit/name == "LunchApp"
  ogit/url
when
  GetMenu
do
//ToDO
  code: code, body: body = action("HTTP", method: "POST", url: ogit/url, body: "LANGUAGE=Englisch&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Mon}&tuesday=${Tue}&wednesday=${Wed}&thursday=${Thu}&friday=${Fri}", insecure: "true")
  log("${ogit/url} => ${body}")
  delete(GetMenu)
  NODE::Var
```

```
result: LOCAL::RESULT = html_extract(html: <html_string>, css_query: <css_query_string>, attribute: <attribute_name>)

**Arguments**

| Position | Keyword | Required | Description |
| :---: | --- | :---: | --- |
| 1 | html | X | The HTML itself as a string |
| 2 | css_query | X | The CSS query of the HTLM elements which we want to extract |
| 3 | attribute | | The name of the attribute which we want to extract.
The value *text* extracts text |

**Return Values**

| Tag | Description |
| --- | --- |
| output | Array which includes matched HTML elements or specific attribute values |
```

#### Create Issues

```
[root@ip-10-4-1-97 ~]# hiro issue --help
usage: hiro [<general options>] issue <subcommand> <specific options/arguments>
Subcommands:
help           show this help
get            retrieve issue(s)
put            create new issue(s)
resolve        set issue state to RESOLVE_EXTERNAL
list           list issue(s)
count          count issue(s)
```

```
[root@ip-10-4-1-97 ~]# hiro issue put --help
usage: hiro [<general options>] issue put [<specific options>] <files to process>
Options
 -h,--help                 show usage
    --input-format <arg>   File format to expect for input files. default: json. supported: json
    --threads <arg>        number of parallel executions
```

-->Try_1
#### Create Issue.SetUserName_UserNode
(just fun --- try machinery)

Issue_SetUserName_UserNode.json
```
{
  "ogit/Automation/originNode": "amerdeevT@arago.de",
  "ogit/subject": "AragoLunchAutomation.Issue_SetUserName_UserNode",
  "/SetUserName": "true"
}
```

```
hiro issue put Issue_SetUserName_UserNode.json
```

```
[root@ip-10-4-1-97 Issue]# hiro issue put Issue_SetUserName_UserNode.json
Issue_SetUserName_UserNode.json -> created with id=cjnen913g3q8wxf663kxgqwae
```

#### Create KI.SetUserName_UserNode

KI_SetUserName_UserNode.ki
```
ki
  id: "KI_SetUserName_UserNode"
on
  ogit/_id
  ogit/firstName
  ogit/lastName
when
  SetUserName@any
do
  UserName = "${ogit/firstName}+${ogit/lastName}"
  log("created UserName ${UserName}")
  delete(SetUserName)
```

```
hiro ki put KI_SetUserName_UserNode.ki
```

```
[root@ip-10-4-1-97 KI]# hiro ki put KI_SetUserName_UserNode.ki
KI_SetUserName_UserNode.ki -> id=cjnef7lx02xlxxf668704eejh updated
```

```
engine audit -d cjnen913g3q8wxf663kxgqwae cjnef7lx02xlxxf668704eejh
```

-->Try_2
#### Create Issue
Issue_SetMenu_User_AndreyMerdeev.json
```
{
  "ogit/Automation/originNode": "amerdeevT@arago.de",
  "ogit/subject": "AragoLunchAutomation.SetMenu_User_AndreyMerdeev",
  "/SelectMenu": "true"
}
```
```
[root@ip-10-4-1-97 Task_02_SetMenu_User_UserName]# hiro issue put Issue_SetMenu_User_AndreyMerdeev.json
Issue_SetMenu_User_AndreyMerdeev.json -> created with id=cjnes63jl47nlxf66bvu88l8g
```

#### Create KI

```
[root@ip-10-4-1-97 Task_02_SetMenu_User_UserName]# hiro ki put KI_SetMenu_User_UserName.ki
KI_SetMenu_User_UserName.ki -> id=cjnerntqh45uaxf66oq6wtti4 updated
```
```
2018-10-18 16:16:49.455 [info] >> SetMenu_User_UserName [cjnerntqh45uaxf66oq6wtti4 v9] => compiled successfully
```







#### Create KI.SetUserName_UserNode

```
hiro issue put Issue_SetMenu_User_ForAll.json
```

```
[root@ip-10-4-1-97 KI]# hiro issue put Issue_SetMenu_User_ForAll.json
Issue_SetMenu_User_ForAll.json -> created with id=cjnbvpcaz0a2kxf6684aa4y42
```

```
hiro issue list
```

```
[root@ip-10-4-1-97 KI]# hiro issue list
cjna2et6m4yfznk66yd26dl4j
cjna4jlyz50j9nk669awtnkzn
cjnbqjhiw044zxf66f85repfx
cjna24mt94y6bnk669pkjswq1
cjna4esxr50c2nk66otazcmjz
cjnae47de59hrnk66r8tczcph
cjna4f0ha50dcnk66zr4ko87g
cjna1i0gj4xjxnk66sb5ex379
cjn50p08113m3nk66mjbr3jws
cjna23qjl4y43nk66chwoi7f5
10 Issues found
```

```
hiro issue get cjnbvpcaz0a2kxf6684aa4y42
```

```
[root@ip-10-4-1-97 Issue]# hiro issue get cjnbvpcaz0a2kxf6684aa4y42
{
  "/SelectMenuForAll": "true",
  "/VNodeTag": "kIc086Y1H3MLDjS81Rrnng==QHlrJlr3hdmF1M8g8KnpgbgruXODLVqa3TJPVz6P9iHT4y9Ce+Y3kNK0A3Ea7hjRklwAGjUOhJIMohc7/mBncGPe81xyeAMae1SoAFQzsYU=",
  "ogit/Automation/deployStatus": "deployed",
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/_created-on": "1539703489931",
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "cjnbvpcaz0a2kxf6684aa4y42",
  "ogit/_is-deleted": "false",
  "ogit/_modified-by": "hiro_engine",
  "ogit/_modified-by-app": "cjix82tev000ou473gko8jgey",
  "ogit/_modified-on": "1539703490453",
  "ogit/_owner": "testusera",
  "ogit/_type": "ogit/Automation/AutomationIssue",
  "ogit/_v": "4",
  "ogit/_v-id": "1539703490453-Y2Vnt5",
  "ogit/status": "PROCESSING",
  "ogit/subject": "AragoLunchAutomation.Issue_SetMenu_User_ForAll"
}
1 Issue found
```

#### Create Issue.SelectMenu_User=UserName

`Issue_SelectMenu_UserName_AndreyMerdeev.json`
```
{
  "ogit/Automation/originNode": "amerdeevT@arago.de",
  "ogit/subject": "AragoLunchAutomation.SelectMenu_User",
  "/SelectMenu": "true"
}
```
```
hiro issue put Issue_SelectMenu_UserName_AndreyMerdeev.json
```

//?Try key --status processing

```
Issue_SelectMenu_UserName_AndreyMerdeev.json -> created with id=cjnae47de59hrnk66r8tczcph
```

```
[root@ip-10-4-1-97 DB_Nodes]# hiro issue get cjnae47de59hrnk66r8tczcph
{
  "/SelectMenu": "true",
  "/VNodeTag": "W1GDFkWPzUBbS5+tdPZ1Pg==OpsQIXhB6F+s//oUfj0YM7ctas3wZvY3bRcCbHW2OUeeEHLotPx4AUKsC25rwfonSgJ7m5lwlIZkdO/1uXLoRK1AWpTMTuu5i11ZqR5UwbI=",
  "ogit/Automation/deployStatus": "undefined variables detected: [/SelectMenu]",
  "ogit/Automation/originNode": "amerdeevT@arago.de",
  "ogit/_created-on": "1539613484113",
  "ogit/_creator": "testusera",
  "ogit/_creator-app": "cjix82rxi000gu473w5kvkpqv",
  "ogit/_graphtype": "vertex",
  "ogit/_id": "cjnae47de59hrnk66r8tczcph",
  "ogit/_is-deleted": "false",
  "ogit/_modified-by": "hiro_engine",
  "ogit/_modified-by-app": "cjix82tev000ou473gko8jgey",
  "ogit/_modified-on": "1539613484709",
  "ogit/_owner": "testusera",
  "ogit/_type": "ogit/Automation/AutomationIssue",
  "ogit/_v": "5",
  "ogit/_v-id": "1539613484709-hCY00c",
  "ogit/status": "WAITING",
  "ogit/subject": "AragoLunchAutomation.SelectMenu_User"
}
1 Issue found
```

#### Create KI.SetMenu_ForAllUsers

//~1~ Action KI
KI_SetMenu_ForAllUsers.ki
```
ki
  id: "SetMenuForAllUsers"
on
  ogit/_id
  ogit/firstName
  ogit/lastName
  with ogit/Software/Application@out:ogit/uses
    Application == "LunchApp"
  end
when
  SelectMenuForAll@any
do
  UserName = "${ogit/firstName}+${ogit/lastName}"
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SelectMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserName}")
```

```
hiro ki put KI_SetMenu_ForAllUsers.ki
```

```
[root@ip-10-4-1-97 KI]# hiro ki put KI_SetMenu_ForAllUsers.ki
KI_SetMenu_ForAllUsers.ki -> created with id=cjnbvbdhe09jkxf66oa0uoiaz
```

```
hiro ki get cjnbvbdhe09jkxf66oa0uoiaz
```

```
[root@ip-10-4-1-97 KI]# hiro ki get cjnbvbdhe09jkxf66oa0uoiaz
ki
  id: "SetMenuForAllUsers"
on
  ogit/_id
  ogit/firstName
  ogit/LastName
  with ogit/Software/Application@out:ogit/uses
    Application == "LunchApp"
  end
when
  SelectMenuForAll@any
do
  UserName = "${ogit/firstName}+${ogit/lastName}"
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SelectMenu = "true", UserName = UserName)
  log("created issue ${ID} for user ${UserName}")

1 KI found
1 undeployed KI found
```

#### Create KI.SelectMenuByProfile



```
hiro ki put KI_SelectMenuByProfile.ki
```

```
[root@ip-10-4-1-97 KI]# hiro ki put KI_SelectMenuByProfile.ki
KI_SelectMenuByProfile.ki -> id=cjnaflan15aw7nk6685oocbzw updated
```

```
hiro ki get cjnaflan15aw7nk6685oocbzw
```

```
[root@ip-10-4-1-97 KI]# hiro ki get cjnaflan15aw7nk6685oocbzw
ki
  id: "SelectMenuByProfile"
on
  ogit/_id
  with ogit/UserMeta/Preferences@out:ogit/likes
    LunchProfileChoice
  end
when
  UserName
  SelectMenu
  not LunchSelected
do
  add(LunchSelected["Mon"] = LunchProfileChoice)
  add(LunchSelected["Tue"] = LunchProfileChoice)
  add(LunchSelected["Wed"] = LunchProfileChoice)
  add(LunchSelected["Thu"] = LunchProfileChoice)
  add(LunchSelected["Fri"] = LunchProfileChoice)

1 KI found
```

#### Create KI.SetMenu_ForUser

//~3~ Action KI
KI_SetMenu_ForUser.ki


```
hiro ki put KI_SetMenu_ForUser.ki
```

```
[root@ip-10-4-1-97 KI]# hiro ki put KI_SetMenu_ForUser.ki
KI_SetMenu_ForUser.ki -> id=cjna37h0c4z61nk669af550b0 updated
```

```
hiro ki get cjna37h0c4z61nk669af550b0
```

```
[root@ip-10-4-1-97 KI]# hiro ki get cjna37h0c4z61nk669af550b0
ki
  id: "SetMenuForUser"
on
  ogit/_id
  with ogit/CustomApplicationData@out:ogit/uses
    Application == "LunchApp"
    AppUrl
  end
when
  UserName
  SelectMenu
  LunchSelected["Mon"] as Mon
  LunchSelected["Tue"] as Tue
  LunchSelected["Wed"] as Wed
  LunchSelected["Thu"] as Thu
  LunchSelected["Fri"] as Fri
do
  code: code, body: body = action("HTTP", method: "POST", url: AppUrl, body: "LANGUAGE=Englisch&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Mon}&tuesday=${Tue}&wednesday=${Wed}&thursday=${Thu}&friday=${Fri}", insecure: "true")
  log("${AppUrl} => ${body}")
  delete(LunchSelected)
  delete(SelectMenu)

1 KI found
```








-->



```
hiro ki put KI_SelectMenuByProfile.ki
```

```
KI_SelectMenuByProfile.ki -> created with id=cjnaflan15aw7nk6685oocbzw
```

```
hiro ki get cjnaflan15aw7nk6685oocbzw
```

```
[root@ip-10-4-1-97 KI]# hiro ki get cjnaflan15aw7nk6685oocbzw
ki
  id: "SelectMenuByProfile"
on
  ogit/_id
  UserName
  with ogit/UserMeta/Preferences@out:ogit/likes
    Type == "LunchProfile"
    list LunchProfile
  end
when
  SelectMenu
  not LunchSelected
do
  LunchSelected = lunchProfileChoice

1 KI found
1 undeployed KI found
```

-->
#### Create Issue SetMenu for User_UserName

```
Issue_SetMenu_UserName_AndreyMerdeev.json
{
  "ogit/Automation/originNode": "amerdeevT@arago.de",
  "ogit/subject": "AragoLunchAutomation.SetMenu_UserName",
  "/SelectMenu": "true"
}
```

ToDo.Done :: What is `ogit/subject` attribute?

```
Issue_SetMenu_UserName_DmitryRuss.json
{
  "ogit/Automation/originNode": "drussT@arago.de",
  "ogit/subject": "AragoLunchAutomation.SetMenu_UserName",
  "/SelectMenu": "true"
}
```

```
hiro issue put Issue_SetMenu_UserName_AndreyMerdeev.json
hiro issue put Issue_SetMenu_UserName_DmitryRuss.json
```

```
Issue_SetMenu_UserName_AndreyMerdeev.json -> created with id=cjna4esxr50c2nk66otazcmjz
Issue_SetMenu_UserName_DmitryRuss.json -> created with id=cjna4f0ha50dcnk66zr4ko87g
```

```
hiro issue list
```

```
[root@ip-10-4-1-97 Issue]# hiro issue list
cjna24mt94y6bnk669pkjswq1
cjn50p08113m3nk66mjbr3jws
cjna1i0gj4xjxnk66sb5ex379
cjna23qjl4y43nk66chwoi7f5
4 Issues found
```

#### Create Issue SetMenu for User_All

Issue to `SetMenu` for `AllUsers`, at node LunchApp `<id>`
```
Issue_SetMenu_UserName_ForAll.json
{
  "ogit/Automation/originNode": "cjpe0fpx10yo27g0209wj9d6b",
  "ogit/subject": "AragoLunchAutomation.SetMenu_ForAllUsers",
  "/SelectMenuForAll": "true"
}
```

Note: global environment --- `Ontology`,
variables `SelectMenu = true` and `SelectMenuForAll = true`

```
hiro issue put Issue_SetMenu_UserName_ForAll.json
```

```
Issue_SetMenu_UserName_ForAll.json -> created with id=cjna4jlyz50j9nk669awtnkzn
```

```
hiro issue list
```

#### Create KI's

#### KI.SetMenuForUser

`#~2~ Decision Making KI`

```
ki
  id: "SelectMenuByProfile"
on
  ogit/_id
  UserName
  with ogit/UserMeta/Preferences@out:ogit/likes
    Type == "LunchProfile"
    LunchProfile
  end
when
  SelectMenu
  not LunchSelected
do
  LunchSelected = LunchProfile
```


#### KI.SetMenuForUser

`#~3~ Action KI`

```
ki
  id: "SetMenuForUser"
on
  ogit/_id
  UserName
  ogit/Software/Application@out:ogit/uses
    Application == "LunchApp"
    AppUrl
  end
when
  SelectMenu
  lunchProfileChoice as Choice
do
  code: code, body: body = action("HTTP", method: "POST", url: AppUrl, body: "LANGUAGE=English&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Choice}&tuesday=${Choice}&wednesday=${Choice}&thursday=${Choice}&friday=${Choice}", insecure: "true")
  log("${AppUrl} => ${body}")
  delete(LunchSelected)
  delete(SelectMenu)
```

Note: use key `insecure: "true"` if need, `default:false`.

```
[root@ip-10-4-1-97 KI]# hiro ki --help
usage: hiro [<general options>] ki <subcommand> <specific options/arguments>

Subcommands:
help           show this help
get            retrieve KI(s)
put            create/update KI(s)
delete         undeploy/delete KI(s)
list           list KI(s)
count          count KI(s)
validate       validate KI (source files)
```

```
[root@ip-10-4-1-97 KI]# hiro ki put --help
usage: hiro [<general options>] ki put  [<specific options>] <files to
            process>
Options
 -h,--help                 show usage
    --input-format <arg>   File format to expect for input files. default:
                           ki. supported: ki
    --show-xids            Include external IDs in output (will change
                           output in batch-mode as well)
    --skip-update          do not try update if KI already exists
    --threads <arg>        number of parallel executions
    --undeploy             do not deploy to engine. Note: if used in
                           conjunction with --allow-update an existing KI
                           will be undeployed from Engine
```

```
hiro ki put KI_SetMenuForUser.ki
```

```
[root@ip-10-4-1-97 KI]# hiro ki put KI_SetMenuForUser.ki
KI_SetMenuForUser.ki -> id=cjna37h0c4z61nk669af550b0 updated
```

```
hiro ki list
```

```
[root@ip-10-4-1-97 KI]# hiro ki list
cjna37h0c4z61nk669af550b0
1 KI found
```

```
audit <issue_id> <ki_id> [<node_id>] --- audit, why ki is not matching on a specific node
```

Try!
```
engine audit cjna23qjl4y43nk66chwoi7f5 cjna37h0c4z61nk669af550b0
engine audit -d cjna23qjl4y43nk66chwoi7f5 cjna37h0c4z61nk669af550b0
```

####

-->
#### Task_SetWeekMenuForUser_UserName
#### Task_GetWeekMenuForUser_UserName
#### Save_WeekMenuForUser_UserName
#### Statistics_WeekMenuForUser_UserName

~1~
`SetMenuForAllUsers.ki`

```
ki
  id: "SetMenuForAllUsers"
on
  ogit/_id
  UserName
  with ogit/CustomApplicationData@out:ogit/uses
    Application == "LunchApp"
  end
when
  SelectMenuForAll@any
do
  issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SelectMenu = "true")
  log("created issue ${ID} for user ${UserName}")
```

~2~
`SelectMenuByProfile.ki`

```
ki
  id: "SelectMenuByProfile"
on
  ogit/_id
  UserName
  with ogit/CustomApplicationData@out:ogit/uses
    Type == "LunchProfile"
    list LunchProfile
  end
when
  SelectMenu
  not LunchSelected
do
  LunchSelected[] = LunchProfile
```

~3~
`SetMenuForUser.ki`

```
ki
  id: "SetMenuForUser"
on
  ogit/_id
  UserName
  with ogit/CustomApplicationData@in:ogit/uses
  #with ogit/CustomApplicationData@out:ogit/uses
    Application == "LunchApp"
    AppUrl
  end
when
  SelectMenu
  list LunchSelected
  LunchSelected["Mon"] as Mon
  LunchSelected["Tue"] as Tue
  LunchSelected["Mon"] as Wed
  LunchSelected["Wed"] as Thu
  LunchSelected["Fri"] as Fri
do
  code: code, body: body = action("HTTP", method: "POST", url: AppUrl, body: "LANGUAGE=Englisch&KW=2&YEAR=2018&USER=${UserName}&MODE=choice_done&monday=${Mon}&tuesday=${Tue}&wednesday=${Wed}&thursday=${Thu}&friday=${Fri}", insecure: "true")
  log("${AppUrl} => ${body}")
  delete(LunchSelected)
  delete(SelectMenu)
```

### Execution

Set Menu for one `user = UserName`

```
engine put issue Issue_SetMenu_UserName_Andrey+Merdeev.json
```

`created issue <id>`

##### -->3
`engine put issue Issue_SetMenu_UserName_Andrey+Merdeev.json`


`engine audit cjk2iwuug1swr7j227epbn0t9 SetMenuForUser cjj8thsnn0i0y6w225p4hgj2x`

And watck the result and logs

```
[root@hirodocker automation_data]# log cjk12hphg1g4w7j225fj7srw7
2018-07-25 11:50:16.202 [info] [coordinator] cjk12hphg1g4w7j225fj7srw7 => initial loading
2018-07-25 11:50:16.203 [info] cjk12hphg1g4w7j225fj7srw7 @  => start loading (pid: #PID<0.26988.45>)
2018-07-25 11:50:16.351 [info] cjk12hphg1g4w7j225fj7srw7 @ cjj8thi1w0i0u6w22v03u5d5w => start processing
2018-07-25 11:50:16.456 [info] cjk12hphg1g4w7j225fj7srw7 >> SelectMenuByProfile [cjj8sh65q0hxr6w22vpzzi37g v7] @ cjj8thi1w0i0u6w22v03u5d5w => execution stats: [exec_time=5954,routed=0,route_time=0,commit_time=40402,bind=0,kis=4,ctx=4,backoff=0,overall_time=105914]
2018-07-25 11:50:16.529 [info] cjk12hphg1g4w7j225fj7srw7 >> SetMenuForUser [cjj8sh66g0hxw6w22ple4a0zc v6] @ cjj8thi1w0i0u6w22v03u5d5w => action executed: POST https://lunch.arago.de/choice.cgi LANGUAGE=Englisch&KW=2&YEAR=2018&USER=Andrey+Merdeev&MODE=choice_done&monday=3&tuesday=3&wednesday=3&thursday=3&friday=3
2018-07-25 11:50:16.529 [info] cjk12hphg1g4w7j225fj7srw7 >> SetMenuForUser [cjj8sh66g0hxw6w22ple4a0zc v6] @ cjj8thi1w0i0u6w22v03u5d5w => https://lunch.arago.de/choice.cgi => <!DOCTYPE html>
2018-07-25 11:50:16.572 [info] cjk12hphg1g4w7j225fj7srw7 >> SetMenuForUser [cjj8sh66g0hxw6w22ple4a0zc v6] @ cjj8thi1w0i0u6w22v03u5d5w => execution stats: [exec_time=72855,routed=0,route_time=0,commit_time=42676,bind=0,kis=4,ctx=4,backoff=0,overall_time=116621]
2018-07-25 11:50:16.613 [info] cjk12hphg1g4w7j225fj7srw7 @ cjj8thi1w0i0u6w22v03u5d5w => resolved
2018-07-25 11:50:16.694 [info] cjk12hphg1g4w7j225fj7srw7 @ cjj8thi1w0i0u6w22v03u5d5w => stop processing (pid: #PID<0.26988.45>)
```

ExecutionTime ~ 1s

Set Menu for all users

```
engine put issue Issue_SetMenu_UserName_ForAll.json
```

`created issue <id>`

And watck the result and logs

```
[root@hirodocker automation_data]# engine put issue Issue_SetMenu_UserName_ForAll.json
cjk12mfhr1gdk7j22idcwof9t: created
[root@hirodocker automation_data]# log cjk12mfhr1gdk7j22idcwof9t
2018-07-25 11:53:56.499 [info] [coordinator] cjk12mfhr1gdk7j22idcwof9t => initial loading
2018-07-25 11:53:56.499 [info] cjk12mfhr1gdk7j22idcwof9t @  => start loading (pid: #PID<0.30134.45>)
2018-07-25 11:53:56.635 [info] cjk12mfhr1gdk7j22idcwof9t @ cjj8tf61w0i0d6w225sh0a21x => start processing
2018-07-25 11:53:56.761 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thsnn0i0y6w225p4hgj2x => issue created: cjk12mfpe1gef7j22v5ex7hrj
2018-07-25 11:53:56.761 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thsnn0i0y6w225p4hgj2x => created issue cjk12mfpe1gef7j22v5ex7hrj for user Dmitry+Russ
2018-07-25 11:53:56.840 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thsnn0i0y6w225p4hgj2x => execution stats: [exec_time=45144,routed=2,route_time=759,commit_time=79672,bind=0,kis=13,ctx=13,backoff=0,overall_time=207129]
2018-07-25 11:53:56.987 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thi1w0i0u6w22v03u5d5w => issue created: cjk12mfu01gf37j22wzonbdlc
2018-07-25 11:53:56.988 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thi1w0i0u6w22v03u5d5w => created issue cjk12mfu01gf37j22wzonbdlc for user Andrey+Merdeev
2018-07-25 11:53:57.088 [info] cjk12mfhr1gdk7j22idcwof9t >> SetMenuForAllUsers [cjj9st3al0n146w22dz2vuyd7 v12] @ cjj8thi1w0i0u6w22v03u5d5w => execution stats: [exec_time=101257,routed=3,route_time=1796,commit_time=101683,bind=0,kis=14,ctx=14,backoff=0,overall_time=250431]
```

ExecutionTime ~ 1s

-->
### A short visualization

#### Graph.Nodes::Circles
#### Graph.Links::Lines
#### KI's::Circles
#### Issues::Stars
#### Engine Grey Box Magic::Square
Task-->Issue InPut
#### Action_Handler::Hand
#### Ontology.EnvVar::Space

[root@hirodocker automation_data]# cd /opt/
arago/        autopilot/    hirodocker/   shiny-server/
[root@hirodocker automation_data]# cd /opt/arago/
conf/        etc/         hiro-engine/
[root@hirodocker automation_data]# cd /opt/arago/hiro-engine/
bin/       docs/      erts-10.0/ lib/       releases/
[root@hirodocker automation_data]# cd /opt/arago/hiro-engine/docs/
CHANGELOG.md  Functions.md  Syntax.md     test-kis/
[root@hirodocker automation_data]# cd /opt/arago/hiro-engine/docs/
[root@hirodocker docs]# less Functions.md


#### Create links between nodes: logic and structure of DB

Node.User_Andrey+Merdeev --> Node.LunchApp
```
engine connect "cjj8thi1w0i0u6w22v03u5d5w" "ogit/uses" "cjj8tf61w0i0d6w225sh0a21x"
```

Node.User_Dmitry+Russ--> Node.LunchApp
```
engine connect "cjj8thsnn0i0y6w225p4hgj2x" "ogit/uses" "cjj8tf61w0i0d6w225sh0a21x"
```

Node_User_Andrey+Merdeev -->  Node_MenuPreferencesProfile_AlwaysThirdChoice
```
engine connect "cjj8thi1w0i0u6w22v03u5d5w" "ogit/uses" "cjj951sv80iy96w222zl1ij14"
```

Node_User_Dmitry+Russ -->  Node_MenuPreferencesProfile_Vegetarian
```
engine connect "cjj8thsnn0i0y6w225p4hgj2x" "ogit/uses" "cjj8tiad70i156w223fhfj8fl"
```

##### -->2
`engine connect "cjj8thsnn0i0y6w225p4hgj2x" "ogit/uses" "cjj8tiad70i156w223fhfj8fl"`
`engine disconnect "cjj5nai8r002p7b22kiin0v2w" "ogit/uses" "cjj5o8d7400dc7b22mxaydhk3"`

##### Delete Link
`engine disconnect "cjj5nai8r002p7b22kiin0v2w" "ogit/uses" "cjj5o8d7400dc7b22mxaydhk3"`




##### -->1
`engine put node /mnt/data/automation_data/Create_Node_User_Andrey+Merdeev.json`
`engine delete node <id>`

```
engine put node /mnt/data/automation_data/Create_Node_User_Andrey+Merdeev.json
```

`created node with <id> cjj8thi1w0i0u6w22v03u5d5w`

```
engine put node /mnt/data/automation_data/Create_Node_User_Dmitry+Russ.json
```

`created node with <id> cjj8thsnn0i0y6w225p4hgj2x`



```
engine put node /mnt/data/automation_data/Create_Node_MenuPreferencesProfile_Vegetarian.json
engine put node /mnt/data/automation_data/Create_Node_MenuPreferencesProfile_AlwaysThirdChoice.json
engine put node /mnt/data/automation_data/Create_Node_MenuPreferencesProfile_InVacation.json
```

got `<id>'s `
`cjj8tiad70i156w223fhfj8fl`
`cjj951sv80iy96w222zl1ij14`
`cjj95bjsf0j7f6w22g16xf0li`




# Hiro KI functions

## General function syntax

**Arguments**

The function arguments are either unnamed ordered ('positional') or named ('keyword'), depending on the called function.

| Type | Example |
| --- | --- |
| Positional | `execute("echo This is a positional argument")` |
| Keyword | `execute(command: "echo This is a keyword argument"`) |

Some function arguments can be stated as either keyword or positional argument. In that case, both are present in the argument description table at the according function description blocks below.

**Return values**

The function results are returned as tagged values, and matched to variables using the syntax:
```
<tag>: <var>,
<tag2>: <var2> [...] = function(...)
```

## Descriptions of implemented functions

The following functions are currently implemented:

### Action

Execute actions as defined by the engine ActionHandler

```
exec: LOCAL::RESULT,
stdout: LOCAL::OUTPUT,
...skipping...
Please see the [elixir documentation](https://hexdocs.pm/elixir/Regex.html) for further details.

```
result: LOCAL::RESULT = regex("Here is a hidden value 47.11 inside", expression: ".*value ([0-9.]+) .*")
```

**Arguments**

| Position | Keyword | Required | Description |
| :---: | --- | :---: | --- |
| 1 |  | X | String onto which the regex is applied |
| | expression | X | Regular Expression, needs exactly one SubMatch-Pattern. e.g. "Value: (0-9+) Seconds" |

**Return Values**

| Tag | Description |
| --- | --- |
| result | Extracted substring |

***

### Sleep

Stops processing for a defined period of time. This function is mainly used for debugging purposes.

```
sleep(time = <seconds>)
sleep(<seconds>)
```

**Arguments**

| Position | Keyword | Required | Description |
| :---: | --- | :---: | --- |
| 1 | time |  | Sleep time in seconds. Default is 10 seconds. |
...skipping...

Extracts specific html elements or just attribute values from those html elements.
```

issueid: LOCAL::ID = create_issue(NodeID = ogit/_id, SelectMenu = "true")
exrtact:

result: LOCAL::RESULT = html_extract(html: <html_string>, css_query: <css_query_string>, attribute: <attribute_name>)
```

**Arguments**

| Position | Keyword | Required | Description |
| :---: | --- | :---: | --- |
| 1 | html | X | The HTML itself as a string |
| 2 | css_query | X | The CSS query of the HTLM elements which we want to extract |
| 3 | attribute | | The name of the attribute which we want to extract.
The value *text* extracts text |

**Return Values**

| Tag | Description |
| --- | --- |
| output | Array which includes matched HTML elements or specific attribute values |
```

### Instance_B :: Prepare Local Run, with the Docker Image

#### Download HIRO container

```
git clone git@github.com:arago/hiro-container.git
```

#### Setup configuration

`run/conf/hiro_engine.conf`

```
ki.loader.filesystem = true
issue.log.file = true
issue.log.stats = true
history.path = graphit
variable.wait_on_undefined = false

routing.nodetypes = ogit/CustomApplicationData
routing.edgetypes = ogit/uses
```

`run/hirodocker.env`

```
# Comment / uncomment needed options
GRAPHIT_MAXMEM=4g
#ERLANG_FLAGS=+S 1:1
CACHE_ONTOLOGY=true
GRAPHIT_IN_MEMORY=true
#ENGINE_DISABLE=true
#GRAPHIT_DISABLE=true
```

### Run HIRO

#### console_01, Start engine

```
IMAGE=hirotm/hiro:latest ./run.sh
```

#### console_02, Going inside docker container

```
docker exec -ti (docker ps -q) bash -c "stty cols $COLUMNS rows $LINES && bash"
```

#### Check engine status

```
engine status
```

### Kill docker process
`bash`
```
docker kill $(docker ps -q)
```

```
docker ps
```

Trouble with the licence: delete `ogit/Attachment` and
Stop&Start engine

```
/opt/hirodocker/engine.sh stop && /opt/hirodocker/engine.sh start
```

#### A)Node.LunchApp, type: ogit/CustomApplicationData

```
Node_LunchApp
{
  type:
    ogit/CustomApplicationData
  attributes:
    mandatory:
    optional:
      /Comment
      /Application
      /AppUrl
  incoming edges:
    ogit/uses <= ogit/CustomApplicationData
}
```

```
Node_LunchApp.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/Comment": "MainNode",
  "/Application": "LunchApp",
  "/AppUrl": "https://lunch.arago.de/choice.cgi"
}
```

Epic fail,
Node.User_Andrey+Merdeev --> Node.LunchApp
```
hiro edge create "cjl7ywra50q4q7622z0ipie0o" "ogit/uses" "cjl6k754k09e67622bolysqeg"
```

```
[root@hirodocker automation_data]# hiro edge create "cjl7ywra50q4q7622z0ipie0o" "ogit/uses" "cjl6k754k09e67622bolysqeg"
ogit/CustomApplicationData is not allowed to have connections to other types
```

Я совсем туплю, в онтологии же тип CustomApplicationData не имеет связей ни с каким другим типом,
так что либо все типы предопределены, либо все типы `ogit/CustomApplicationData`,
по второму пункту полный провал, больше понимания, но по факту ничего нельзая пока "улучшить".

```
[root@hirodocker automation_data]# engine connect "cjl7ywra50q4q7622z0ipie0o" "ogit/uses" "cjl6k754k09e67622bolysqeg"
edge: {400, %{"message" => "ogit/CustomApplicationData is not allowed to have connections to other types"}}
```


#### A)Node.User_UserName, type: ogit/CustomApplicationData

```
Node_User_UserName
{
  type:
    ogit/CustomApplicationData
  attributes:
    optional:
      /UserName
  incoming edges:
    ogit/uses <= ogit/CustomApplicationData
  outgoing edges:
    ogit/uses => ogit/Software/Application
    ogit/tracks => ogit/Timeseries
}
```

```
Node_User_Andrey+Merdeev.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/UserName": "Andrey+Merdeev"
}
```

```
Node_User_Dmitry+Russ.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/UserName": "Dmitry+Russ"
}
```

Does not work now!
1) `ogit/email` attribute is mandatory, need toFix Ontology description
2) does not work in docker instance ToHelp, works in a full build with generated `id==email`
(ToHelp, but need VPN to access to internal services of a company)

#### Download hiro-clients for the docker image: HIRO-CLI

```
git clone git@github.com:arago/hiro-clients.git
```

~OR~

```
Node_MenuPreferencesProfile_Choice
{
  type:
    ogit/CustomApplicationData
  attributes:
    optional:
      "/Type": "LunchProfile",
      "/LunchProfile": {"Mon": "N", "Tue": "N", "Wed": "N", "Thu": "N", "Fri": "N"}, N = {1,2,3,4}
  outgoing edges:
    ogit/uses => git/CustomApplicationData
}
```

```
Node_MenuPreferencesProfile_Vegetarian.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/Type": "LunchProfile",
  "/LunchProfile": {"Mon": "2", "Tue": "2", "Wed": "2", "Thu": "2", "Fri": "2"}
}
```

```
Node_MenuPreferencesProfile_AlwaysThirdChoice.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/Type": "LunchProfile",
  "/LunchProfile": {"Mon": "3", "Tue": "3", "Wed": "3", "Thu": "3", "Fri": "3"}
}
```

```
Node_MenuPreferencesProfile_InVacation.json
{
  "ogit/_type": "ogit/CustomApplicationData",
  "/Type": "LunchProfile",
  "/LunchProfile": {"Mon": "4", "Tue": "4", "Wed": "4", "Thu": "4", "Fri": "4"}
}
```


```
So, like Einstein says
Everything Should Be Made as Simple as Possible, But Not Simpler

my suggestion wait with the extending Ontology with the entity Social,
instead keep it simple, as we already have all necessary parts in the current Ontology.

How do you think about the following architecture:
ogit:Person {rates} ogit:Organization
user_1 {likes/rates, weigh=5} Arago.Cantine [..., free--attributes:Menu=1]
user_2 {likes/rates, weigh=5} Arago.Cantine [..., free--attributes:Menu=Vegetarian]

It should work perfectly fine.

May be verbs likes/rates should be extended for the links between Person and Organization, which somehow makes sense.
```
