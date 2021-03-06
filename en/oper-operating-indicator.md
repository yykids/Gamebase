## Game > Gamebase > Console Guide > Monitoring

Status of app users is shown by indicator or graph.
The menu is composed of Monitoring, Concurrent Group User, User Statistics, Install URL Statistics, and Sales Status.


## Monitoring
![operation-indicator_01_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_01_201812_en.png)
Shows statistics of current app users, reserved push status at the moment, and reserved maintenance.
The screen is automatically refreshed every five minutes, to display changed indicators in real time.

* 기본 지표
	* CCU(concurrent connected users): 실시간 동시 접속자 수
	* MCU(maximum concurrent users): 하루 동안의 최대 동시 접속자 수(실시간, 일자별 조회 가능)
	* DAU(daily active users): 하루 동안 게임을 사용한 순 이용자 수(실시간, 일자별 조회 가능)
	* NRU(new registered users): 하루 동안의 신규 사용자 수(실시간, 일자별 조회 가능)
* 점유율 차트: 게임 사용자의 점유율 파이 차트
	* 운영체제별: Android, iOS, WebGL 등
	* 국가별: SDK에서 수집된 USIM 국가 기준
	* 버전별: Console에 등록된 클라이언트 버전별 점유율
* 동시 접속 변화 그래프: 금일 00:00부터 현재 시간까지의 동시 접속 변화 그래프
	* 점검 및 푸시에 따른 동시 접속 변화를 확인하기 쉽도록 점검, 푸시 내역을 그래프에 별도로 표시하고 있습니다.
	* 관련데이터는 검색조건 우측의 버튼을 통해 다운로드를 받을 수 있습니다.
    

* Basic Indicators
    * Concurrent Connected User (CCU): Number of concurrently connected users in real time
    * Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date)
    * Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date)
    * New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date)
* Market Share Chart: Pie Chart of Games User's Market Share
    * By Operating System: Android, iOS, WebGL, and etc.
    * By Country: By USIM country collected by SDK
    * By Version: By client version registered at console
* Change Graph of Concurrent Connection: Graph of changes of concurrent connection from 00:00 today up to now
	* Show maintenance and push details on graph to easily follow changes in concurrent connection out of maintenance and push.
  * You can download the graph in .xlsx or .csv format by clicking on the button at the top right corner of the panel.

## User Statistics
![operation-indicator_02_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_02_201812_en.png)
Check DAU, MCU,NRU, and CCU AVG on graphs.
Easy to identify trend changes of the current games users.
You can specify a date by using the selection bar at the top right to check data.
Each item is described as below:

* Description
    * Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date)
    * Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date)
    * New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date)
    * Concurrent Connected User Average(CCU AVG): Average of concurrently connected users in real time

## Concurrent Group User
![operation-indicator_03_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_03_201812_en.png)
Check statistics of concurrent connection of groups where your project belongs to. The real-time number of concurrent connectors can be seen at a glance per operating system of many projects at your authority.


## Installed URL Statistics
![operation-indicator_04_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_04_201812_en.png)
Shows statistical data of install URL calls.

* Change graph of installed URL calls by date
* Market share by browser: Internet Explorer, Chrome, and etc.
* Market share by platform: Android, iOS, and etc.


## Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Statistics1_1.2.png)
Shows real-time sales statistics of a game in Sales Status.
Select a currency to check sales volume of the currency.

#### (1) Statistical Graph of Daily Sales Status
Easy to identify daily sales status and trends on a line graph.

#### (2) Monthly Sales Status
Displays monthly or accumulated sales of the month in statistics by store and aggregated data.

#### (3) Daily Sales Status
Retrieves sales status of each store registered in an app, by date.
You can find data up to today for the month.
Daily sales status is more precise than monthly status. You can compare and analyze sales status, during an event or when an issue occurs.