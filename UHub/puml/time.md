# Timing Diagram

A [Timing Diagram](https://en.wikipedia.org/wiki/Timing_diagram_%28Unified_Modeling_Language%29) in UML is a specific type of **interaction diagram** that visualizes the **timing constraints** of a system. It focuses on the **chronological order of events**, showcasing how different objects interact with each other over time. **Timing diagrams** are especially useful in **real-time systems** and **embedded systems** to understand the behavior of objects throughout a given period.
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Declaring element or participant

You declare participant using the following keywords, depending on how you want them to be drawn.

| **Keyword** | **Description** |
| --- | --- |
| `analog` | An `analog` signal is continuous, and the values are linearly interpolated between the given setpoints |
| `binary` | A `binary` signal restricted to only 2 states |
| `clock` | A `clocked` signal that repeatedly transitions from high to low, with a `period`, and an optional `pulse` and `offset` |
| `concise` | A simplified `concise` signal designed to show the movement of data (great for messages) |
| `robust` | A `robust` complex line signal designed to show the transition from one state to another (can have many states) |

You define state change using the `@` notation, and the `is` verb.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle  @100 WU is Waiting WB is Processing  @300 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-f80bc9b939fb941ac6a82224632409a4.png) |
| --- | --- |

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock   "Clock_0"   as C0 with period 50 clock   "Clock_1"   as C1 with period 50 pulse 15 offset 10 binary  "Binary"  as B concise "Concise" as C robust  "Robust"  as R analog  "Analog"  as A   @0 C is Idle R is Idle A is 0  @100 B is high C is Waiting R is Processing A is 3  @300 R is Waiting A is 1 @enduml   `    ![](https://plantuml.com/imgw/img-93bfa59af17846bada72c2fc4b751c7f.png) |
| --- | --- |

*[Ref. [QA-14631](https://forum.plantuml.net/14631), [QA-14647](https://forum.plantuml.net/14647) and [QA-11288](https://forum.plantuml.net/11288/mixed-signal-timing-diagram)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Binary and Clock

It's also possible to have binary and clock signal, using the following keywords:

- `binary`

- `clock`

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock clk with period 1 binary "Enable" as EN  @0 EN is low  @5 EN is high  @10 EN is low @enduml   `    ![](https://plantuml.com/imgw/img-bdcb3725605fa302c195a549a51f9d9b.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Adding message

You can add message using the following syntax.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle  @100 WU -> WB : URL WU is Waiting WB is Processing  @300 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-f80ba5f0d61f09ea0ba54b36ba7eb60d.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Relative time

It is possible to use relative time with `@`.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "DNS Resolver" as DNS robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle DNS is Idle  @+100 WU -> WB : URL WU is Waiting WB is Processing  @+200 WB is Waiting WB -> DNS@+50 : Resolve URL  @+100 DNS is Processing  @+300 DNS is Idle @enduml   `    ![](https://plantuml.com/imgw/img-4e191d552feca2d42b4f5d50eb8d9d9c.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Anchor Points

Instead of using absolute or relative time on an absolute time you can define a time as an anchor point by using the `as` keyword and starting the name with a `:`.

```
@XX as :<anchor point name>

```

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock clk with period 1 binary "enable" as EN concise "dataBus" as db  @0 as :start @5 as :en_high  @10 as :en_low @:en_high-2 as :en_highMinus2  @:start EN is low db is "0x0000"  @:en_high EN is high  @:en_low EN is low  @:en_highMinus2 db is "0xf23a"  @:en_high+6 db is "0x0000" @enduml   `    ![](https://plantuml.com/imgw/img-1ab3af323cfcc1dc145fef84782c4c30.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Participant oriented

Rather than declare the diagram in chronological order, you can define it by participant.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @WB 0 is idle +200 is Proc. +100 is Waiting  @WU 0 is Waiting +500 is ok @enduml   `    ![](https://plantuml.com/imgw/img-7a914796d81f74dc0e4f9ed4097c87ad.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Setting scale

You can also set a specific scale.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml concise "Web User" as WU scale 100 as 50 pixels  @WU 0 is Waiting +500 is ok @enduml   `    ![](https://plantuml.com/imgw/img-ccdc91e2d8661e2cb97c394ca93aa635.png) |
| --- | --- |

When using absolute Times/Dates, 1 "tick" is equivalent to 1 second.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml concise "Season" as S '30 days is scaled to 50 pixels scale 2592000 as 50 pixels  @2000/11/01 S is "Winter"  @2001/02/01 S is "Spring"  @2001/05/01 S is "Summer"  @2001/08/01 S is "Fall" @enduml   `    ![](https://plantuml.com/imgw/img-7c2a8c55c0b07d16043741163f1ed8ad.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Initial state

You can also define an inital state.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  WB is Initializing WU is Absent  @WB 0 is idle +200 is Processing +100 is Waiting  @WU 0 is Waiting +500 is ok @enduml   `    ![](https://plantuml.com/imgw/img-03a2bed29328f54ea861cef479988da5.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Intricated state

A signal could be in some undefined state.
### Intricated or undefined robust state

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Signal1" as S1 robust "Signal2" as S2 S1 has 0,1,2,hello S2 has 0,1,2 @0 S1 is 0 S2 is 0 @100 S1 is {0,1} #SlateGrey S2 is {0,1} @200 S1 is 1 S2 is 0 @300 S1 is hello S2 is {0,2} @enduml   `    ![](https://plantuml.com/imgw/img-550592c55c78d508fc7404b275c2d450.png) |
| --- | --- |

### Intricated or undefined binary state

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock "Clock" as C with period 2 binary "Enable" as EN  @0 EN is low @1 EN is high @3 EN is low @5 EN is {low,high} @10 EN is low @enduml   `    ![](https://plantuml.com/imgw/img-5616f2f3d472d64435ec291adc962f4d.png) |
| --- | --- |

*[Ref. [QA-11936](https://forum.plantuml.net/11936) and [QA-15933](https://forum.plantuml.net/15933)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Hidden state

It is also possible to hide some state.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml concise "Web User" as WU  @0 WU is {-}  @100 WU is A1  @200 WU is {-}  @300 WU is {hidden}  @400 WU is A3  @500 WU is {-} @enduml   `    ![](https://plantuml.com/imgw/img-4070841664fd1f05e1b4bcab445510c7.png) |
| --- | --- |

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml scale 1 as 50 pixels  concise state0 concise substate1 robust bit2  bit2 has HIGH,LOW  @state0 0 is 18_start 6 is s_dPause 8 is 10_data 14 is {hidden}  @substate1 0 is sSeq 4 is sPause 6 is {hidden} 8 is dSeq 12 is dPause 14 is {hidden}  @bit2 0 is HIGH 2 is LOW 4 is {hidden} 8 is HIGH 10 is LOW 12 is {hidden} @enduml   `    ![](https://plantuml.com/imgw/img-138cc7f191f325751c30fbd78d94e912.png) |
| --- | --- |

*[Ref. [QA-12222](https://forum.plantuml.net/12222)]*

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Hide time axis

It is possible to hide time axis.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml hide time-axis concise "Web User" as WU  WU is Absent  @WU 0 is Waiting +500 is ok @enduml   `    ![](https://plantuml.com/imgw/img-8a96a3cb3ee343d389a5d641d6909854.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Using Time and Date

It is possible to use time or date.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @2019/07/02 WU is Idle WB is Idle  @2019/07/04 WU is Waiting : some note WB is Processing : some other note  @2019/07/05 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-74d767d09a5825c68478611e9f288cc9.png) |
| --- | --- |

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @1:15:00 WU is Idle WB is Idle  @1:16:30 WU is Waiting : some note WB is Processing : some other note  @1:17:30 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-da34b4d4cc394f479177a59fdeb3adab.png) |
| --- | --- |

*[Ref. [QA-7019](https://forum.plantuml.net/7019/hh-mm-ss-time-format-in-timing-diagram)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Change Date Format

It is also possible to change date format.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  use date format "YY-MM-dd"  @2019/07/02 WU is Idle WB is Idle  @2019/07/04 WU is Waiting : some note WB is Processing : some other note  @2019/07/05 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-19a1602153eaabd983013d0a5a0f339b.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Manage time axis labels

You can manage the time-axis labels.
### Label on each tick *(by default)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml scale 31536000 as 40 pixels use date format "yy-MM"  concise "OpenGL Desktop" as OD  @1992/01/01 OD is {hidden}  @1992/06/30 OD is 1.0  @1997/03/04 OD is 1.1  @1998/03/16 OD is 1.2  @2001/08/14 OD is 1.3  @2004/09/07 OD is 3.0  @2008/08/01 OD is 3.0  @2017/07/31 OD is 4.6  @enduml   `    ![](https://plantuml.com/imgw/img-6fbb889f3539e2dfb94302a3e7eb72c1.png) |
| --- | --- |

### Manual label *(only when the state changes)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml scale 31536000 as 40 pixels  manual time-axis use date format "yy-MM"  concise "OpenGL Desktop" as OD  @1992/01/01 OD is {hidden}  @1992/06/30 OD is 1.0  @1997/03/04 OD is 1.1  @1998/03/16 OD is 1.2  @2001/08/14 OD is 1.3  @2004/09/07 OD is 3.0  @2008/08/01 OD is 3.0  @2017/07/31 OD is 4.6  @enduml   `    ![](https://plantuml.com/imgw/img-ff054a0ebcbb8b468d0e796b966e2c69.png) |
| --- | --- |

*[Ref. [GH-1020](https://github.com/plantuml/plantuml/issues/1020)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Adding constraint

It is possible to display time constraints on the diagrams.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  WB is Initializing WU is Absent  @WB 0 is idle +200 is Processing +100 is Waiting WB@0 <-> @50 : {50 ms lag}  @WU 0 is Waiting +500 is ok @200 <-> @+150 : {150 ms} @enduml   `    ![](https://plantuml.com/imgw/img-ca073ec2feda2bd728a74d341d32ca7b.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Highlighted period

You can higlight a part of diagram.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle  @100 WU -> WB : URL WU is Waiting #LightCyan;line:Aqua  @200 WB is Proc.  @300 WU -> WB@350 : URL2 WB is Waiting  @+200 WU is ok  @+200 WB is Idle  highlight 200 to 450 #Gold;line:DimGrey : This is my caption highlight 600 to 700 : This is another\nhighlight @enduml   `    ![](https://plantuml.com/imgw/img-531ffb4699b7eda6c603b0ae7004edb4.png) |
| --- | --- |

*[Ref. [QA-10868](https://forum.plantuml.net/10868/highlighted-periods-in-timing-diagrams)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Using notes

You can use the `note top of` and `note bottom of` keywords to define notes related to a single object or participant *(available only for *`concise`* or *`binary`* object).*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle  @100 WU is Waiting WB is Processing note top of WU : first note\non several\nlines note bottom of WU : second note\non several\nlines  @300 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-e384dfc4487f3092f38c078c47ba0124.png) |
| --- | --- |

*[Ref. [QA-6877](https://forum.plantuml.net/6877), [GH-1465](https://github.com/plantuml/plantuml/issues/1465)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Adding texts

You can optionally add a title, a header, a footer, a legend and a caption:

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml Title This is my title header: some header footer: some footer legend Some legend end legend caption some caption  robust "Web Browser" as WB concise "Web User" as WU  @0 WU is Idle WB is Idle  @100 WU is Waiting WB is Processing  @300 WB is Waiting @enduml   `    ![](https://plantuml.com/imgw/img-351d017ea25f5e4ae5d90d85a8fb2ee1.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Complete example

Thanks to [Adam Rosien](https://twitter.com/arosien) for this example.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml concise "Client" as Client concise "Server" as Server concise "Response freshness" as Cache  Server is idle Client is idle  @Client 0 is send Client -> Server@+25 : GET +25 is await +75 is recv +25 is idle +25 is send Client -> Server@+25 : GET\nIf-Modified-Since: 150 +25 is await +50 is recv +25 is idle @100 <-> @275 : no need to re-request from server  @Server 25 is recv +25 is work +25 is send Server -> Client@+25 : 200 OK\nExpires: 275 +25 is idle +75 is recv +25 is send Server -> Client@+25 : 304 Not Modified +25 is idle  @Cache 75 is fresh +200 is stale @enduml   `    ![](https://plantuml.com/imgw/img-242e5e98d3e1799ef2cabd63e896cd29.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Digital Example

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml scale 5 as 150 pixels  clock clk with period 1 binary "enable" as en binary "R/W" as rw binary "data Valid" as dv concise "dataBus" as db concise "address bus" as addr  @6 as :write_beg @10 as :write_end  @15 as :read_beg @19 as :read_end   @0 en is low db is "0x0" addr is "0x03f" rw is low dv is 0  @:write_beg-3  en is high @:write_beg-2  db is "0xDEADBEEF" @:write_beg-1 dv is 1 @:write_beg rw is high   @:write_end rw is low dv is low @:write_end+1 rw is low db is "0x0" addr is "0x23"  @12 dv is high @13  db is "0xFFFF"  @20 en is low dv is low @21  db is "0x0"  highlight :write_beg to :write_end #Gold:Write highlight :read_beg to :read_end #lightBlue:Read  db@:write_beg-1 <-> @:write_end : setup time db@:write_beg-1 -> addr@:write_end+1 : hold @enduml   `    ![](https://plantuml.com/imgw/img-0b0d89f7209370fc43389425de53990a.png) |
| --- | --- |

## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Adding color

You can add [color](https://plantuml.com/color).

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml concise "LR" as LR concise "ST" as ST  LR is AtPlace #palegreen ST is AtLoad #gray  @LR 0 is Lowering 100 is Lowered #pink 350 is Releasing   @ST 200 is Moving @enduml   `    ![](https://plantuml.com/imgw/img-35ae177bae8a249c52dc0eba7d5d4f35.png) |
| --- | --- |

*[Ref. [QA-5776](https://forum.plantuml.net/5776)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Using (global) style

### Without style *(by default)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU  WB is Initializing WU is Absent  @WB 0 is idle +200 is Processing +100 is Waiting WB@0 <-> @50 : {50 ms lag}  @WU 0 is Waiting +500 is ok @200 <-> @+150 : {150 ms} @enduml   `    ![](https://plantuml.com/imgw/img-ca073ec2feda2bd728a74d341d32ca7b.png) |
| --- | --- |

### With style

You can use [style](https://plantuml.com/style-evolution) to change rendering of elements.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml <style> timingDiagram {   document {     BackGroundColor SandyBrown   }  constraintArrow {   LineStyle 2-1   LineThickness 3   LineColor Blue  } } </style> robust "Web Browser" as WB concise "Web User" as WU  WB is Initializing WU is Absent  @WB 0 is idle +200 is Processing +100 is Waiting WB@0 <-> @50 : {50 ms lag}  @WU 0 is Waiting +500 is ok @200 <-> @+150 : {150 ms} @enduml   `    ![](https://plantuml.com/imgw/img-964b7feddfb6e9e08d9758e82ca444a0.png) |
| --- | --- |

*[Ref. [QA-14340](https://forum.plantuml.net/14340/color-of-arrow-in-timing-diagram)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Applying Colors to specific lines

You can use the `<style>` tags and sterotyping to give a name to line attributes.

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml <style> timingDiagram {   .red {     LineColor red   }   .blue {     LineColor blue     LineThickness 5   } } </style>  clock clk with period 1 binary "Input Signal 1"  as IS1 binary "Input Signal 2"  as IS2 <<blue>> binary "Output Signal 1" as OS1 <<red>>  @0 IS1 is low IS2 is high OS1 is low @2 OS1 is high @4 OS1 is low @5 IS1 is high OS1 is high @6 IS2 is low @10 IS1 is low OS1 is low @enduml   `    ![](https://plantuml.com/imgw/img-a0f8e99b6ca56c7e958d696b2e8f6c56.png) |
| --- | --- |

*[[Ref. QA-15870](https://forum.plantuml.net/15870/timing-diagram-assign-different-colors-single-participants?show=15870#q15870)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Compact mode

You can use `compact` command to compact the timing layout.
### By default

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Web Browser" as WB concise "Web User" as WU robust "Web Browser2" as WB2  @0 WU is Waiting WB is Idle WB2 is Idle  @200 WB is Proc.  @300 WB is Waiting WB2 is Waiting  @500 WU is ok  @700 WB is Idle @enduml   `    ![](https://plantuml.com/imgw/img-f769d5decec1ddba6cd2e36549c6392e.png) |
| --- | --- |

#### Global mode with `mode compact`

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml mode compact robust "Web Browser" as WB concise "Web User" as WU robust "Web Browser2" as WB2  @0 WU is Waiting WB is Idle WB2 is Idle  @200 WB is Proc.  @300 WB is Waiting WB2 is Waiting  @500 WU is ok  @700 WB is Idle @enduml   `    ![](https://plantuml.com/imgw/img-5026969bcec65aec83a31659d7faf9e6.png) |
| --- | --- |

### Local mode with only `compact` on element

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml compact robust "Web Browser" as WB compact concise "Web User" as WU robust "Web Browser2" as WB2  @0 WU is Waiting WB is Idle WB2 is Idle  @200 WB is Proc.  @300 WB is Waiting WB2 is Waiting  @500 WU is ok  @700 WB is Idle @enduml   `    ![](https://plantuml.com/imgw/img-eb834d20d04d005364a2594f1f57c991.png) |
| --- | --- |

*[Ref. [QA-11130](https://forum.plantuml.net/11130/is-there-a-compact-timing-diagram)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Scaling analog signal

You can scale analog signal.
### Without scaling: 0-max *(by default)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml title Between 0-max (by default) analog "Analog" as A  @0 A is 350  @100 A is 450  @300 A is 350 @enduml   `    ![](https://plantuml.com/imgw/img-af87677f66bb9299e7309198bb20e16a.png) |
| --- | --- |

### With scaling: min-max

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml title Between min-max analog "Analog" between 350 and 450 as A  @0 A is 350  @100 A is 450  @300 A is 350 @enduml   `    ![](https://plantuml.com/imgw/img-1f00450b3a77c76fbe1155fdff12d82f.png) |
| --- | --- |

*[Ref. [QA-17161](https://forum.plantuml.net/17161/timing-diagram-better-scaling-of-analog-values)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Customise analog signal

### Without any customisation *(by default)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml analog "Vcore" as VDD analog "VCC" as VCC  @0 VDD is 0 VCC is 3 @2 VDD is 0 @3 VDD is 6 VCC is 6 VDD@1 -> VCC@2 : "test" @enduml   `    ![](https://plantuml.com/imgw/img-3eaf8858fadd79b0891b7227e729337b.png) |
| --- | --- |

### With customisation (on scale, ticks and height)

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml analog "Vcore" as VDD analog "VCC" between -4.5 and 6.5 as VCC VCC ticks num on multiple 3 VCC is 200 pixels height  @0 VDD is 0 VCC is 3 @2 VDD is 0 @3 VDD is 6 VCC is 6 VDD@1 -> VCC@2 : "test" @enduml   `    ![](https://plantuml.com/imgw/img-bf278dbff9a068a927ce95f1ca5bfac6.png) |
| --- | --- |

*[Ref. [QA-11288](https://forum.plantuml.net/11288/mixed-signal-timing-diagram?show=11397#c11397)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Order state of robust signal

### Without order *(by default)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Flow rate" as rate  @0 rate is high  @5 rate is none  @6 rate is low @enduml   `    ![](https://plantuml.com/imgw/img-afc4359f32a66de120b668a12cf1767e.png) |
| --- | --- |

### With order

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Flow rate" as rate rate has high,low,none  @0 rate is high  @5 rate is none  @6 rate is low @enduml   `    ![](https://plantuml.com/imgw/img-ace470303a6f1a83f3fc8bac7fecdac2.png) |
| --- | --- |

### With order and label

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml robust "Flow rate" as rate rate has "35 gpm" as high rate has "15 gpm" as low rate has "0 gpm" as none  @0 rate is high  @5 rate is none  @6 rate is low @enduml   `    ![](https://plantuml.com/imgw/img-3b30e8aacdddcb55d45b19574ea98aaf.png) |
| --- | --- |

*[Ref. [QA-6651](https://forum.plantuml.net/6651/order-of-states-in-timing-diagram)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Defining a timing diagram

### By Clock *(@clk)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock "clk" as clk with period 50 concise "Signal1" as S1 robust "Signal2" as S2 binary "Signal3" as S3   @clk*0 S1 is 0 S2 is 0  @clk*1 S1 is 1 S3 is high  @clk*2 S3 is down  @clk*3 S1 is 1 S2 is 1 S3 is 1  @clk*4 S3 is down @enduml   `    ![](https://plantuml.com/imgw/img-a61f59b551e4d956c4d6d3d58032b542.png) |
| --- | --- |

### By Signal *(@S)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock "clk" as clk with period 50 concise "Signal1" as S1 robust "Signal2" as S2 binary "Signal3" as S3  @S1 0 is 0 50 is 1 150 is 1  @S2 0 is 0 150 is 1  @S3 50  is 1 100 is low 150 is high 200 is 0 @enduml   `    ![](https://plantuml.com/imgw/img-453274cd577e164ca9192911206ce221.png) |
| --- | --- |

### By Time *(@time)*

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml clock "clk" as clk with period 50 concise "Signal1" as S1 robust "Signal2" as S2 binary "Signal3" as S3  @0 S1 is 0 S2 is 0  @50 S1 is 1 S3 is 1  @100 S3 is low  @150 S1 is 1 S2 is 1 S3 is high  @200 S3 is 0 @enduml   `    ![](https://plantuml.com/imgw/img-18d37a7f3e0971881ac17fa68a55bb09.png) |
| --- | --- |

*[Ref. [QA-9053](https://forum.plantuml.net/9053/timing-diagrams-for-binary-signal-and-data-buses?show=9057#a9057)]*
## ![](https://plantuml.com/backtop1.svg "Back to top")

![](https://plantuml.com/edit1.svg)

Annotate signal with comment

| ![](https://plantuml.com/clipboard1.svg "Copy to clipboard")<br>![](https://plantuml.com/edit1.svg "Edit online") | `  @startuml binary "Binary Serial Data" as D robust "Robust" as R concise "Concise" as C  @-3 D is low: idle R is lo: idle C is 1: idle @-1 D is high: start R is hi: start C is 0: start  @0 D is low: 1 lsb R is lo: 1 lsb C is 1: lsb  @1 D is high: 0 R is hi: 0 C is 0  @6 D is low: 1 R is lo: 1 C is 1  @7 D is high: 0 msb R is hi: 0 msb C is 0: msb  @8 D is low: stop R is lo: stop C is 1: stop  @0 <-> @8 : Serial data bits for ASCII "A" (Little Endian) @enduml   `    ![](https://plantuml.com/imgw/img-3a8fc8c8e07fb6552901006abd4f249e.png) |
| --- | --- |

*[Ref. [QA-15762](https://forum.plantuml.net/15762/annotate-binary-waveforms), and [QH-888](https://github.com/plantuml/plantuml/issues/888)]*