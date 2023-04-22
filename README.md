# Triviador



## Build on Windows

- Prefered Windows OS: **Windows 10 x64**

required software:
1) visual studio 2019
		- go to tools --> get tools and features --> individual components or look at installation details
		- make sure that the following options are selected:
			1) **MSVC v142 - VS 2019 C++ x64/x84 build tools**
			2) **C++ CMake tools for Windows**
2) CMake (minimum 3.14). Downloadable [here](https://cmake.org/download/)
3) vcpkg --> [here](https://vcpkg.io/en/getting-started.html)
4) Qt 6.4.0 (Qt online installer) [here](https://www.qt.io/download-qt-installer?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4)


#### Steps for building
1) configure vcpkg
	 -  in C: (or your main drive) create a directory called "dev"
	 - Clone the repo (you can do it any way you want)
		 - Open git bash, go to "C:/dev" and execute 
		 - `git clone https://github.com/Microsoft/vcpkg.git`
		 - `cd vcpkg`
		 - `./vcpkg/bootstrap-vcpkg.bat`
		 - `./vpckg integrate install`
	 - In the search box write "Edit the system environment variables", click it -->Environment Variables
		 - In user variables select Path and click edit --> New --> type "C:\dev\vcpkg" --> click Ok
 		 - At user variables click New
			 - Variable Name: "VCPKG_DEFAULT_TRIPLET"
			 - Variable Value: "x64-windows"
			 - click Ok
		 - Anywhere in git bash or cmd, execute: <br>
		 	-`vcpkg install sqlite-orm` (if this does not work, you may need to open the cmd line again)<br>
			-`vcpkg install crow`<br>
			-`vcpkg install cpr`
 2) Install Qt6 
	 1) download Qt online installer
	 2) You'll need to create an account, the gui will prompt that.
	 3) Select installation folder C:\dev\Qt (or anywhere you want)
	 4) Select custom installation
	 5) Collapse Qt and select:
		 1) MSVC 2019 64-bit
		 2) MinGW 11.2.0 64-bit
	 6) Finish the installation  (this will take a while)
	 7) go to environment variables
	 8) At system variables click New
		 1) Variable name: "QTDIR"
		 2) Variable value: Select the path to the compiler (browse directory) --> C:\dev\Qt\6.4.0\msvc2019_64
		 3) Select the Path variable, click edit and add 2 new values
			 1) %QTDIR%\lib
			 2) %QTDIR%\bin
		 4) Restart the computer
3) Build project
	1) Install CMake
	2) Open cmake-gui (search it in the box)
		1) where is the source code: add the path to the cloned repository
		2) where to build the binaries: add the path to the cloned repository/build
	3) click configure
		1) Select "Visual Studio 16 2019" as generator
		2) Select x64
		3) click finish
	4) generate --> open project
	5) build



#### Alternative IDE: **CLION**
1) same building steps 
2) When defining QTDIR environment variable give path to mingw_64 --> C:\dev\Qt\6.4.0\mingw_64
3) In CLION --> settings --> Build, Execution, Deployment --> CMake
	- In CMake options add: "-DCMAKE_TOOLCHAIN_FILE=C:/dev/vcpkg/scripts/buildsystems/vcpkg.cmake
4) Right click on the main CMakeLists.txt --> Load CMake Project

## Prezentare generală

Acest proiect implementeaza o aplicație desktop multiplayer interactivă cu componente Modern C ++, construită folosind o varietate de tehnologii precum: cadru Qt pentru design front-end, cadru Crow pentru operațiuni pe partea de server, SQLite ORM ca un instrument backend pentru gestionarea eficientă a bazelor de date.

Aplicația oferă o serie de caracteristici concepute pentru a îmbunătăți experiența de joc, inclusiv meciuri multiplayer personalizate în timp real, o bază de date cuprinzătoare trivia și algoritmi de matchmaking sofisticați.

Jocurile de inteligență și strategie sunt populare în rândul copiilor, dar și al adulților care se mandresc cu o vastă cultură generală. Un astfel de tip de joc este Triviador, iar scopul acestui proiect este de a implementa propria noastra versiune a acestui joc. Pentru implementarea aplicației trebuie să respectăm regulile jocului:

Acțiunea jocului se desfășoară în cadrul unei hărți și este asemenea unei bătălii pentru teritoriu din cadrul unui război, obiectivul fiind cel de cucerire a unei suprafețe cat mai mari prin luptă.
2.“Lupta” între 2 jucători se realizeaza prin intermediul întrebărilor de cultura generala, iar întrebările pot fi 2 tipuri:

a. întrebări de tip grila cu un singur răspuns corect: acest tip de întrebări decide castigatorul doar dacă unul dintre jucătorii răspunde corect

b. întrebări cu răspuns numeric: aceste întrebări decid castigatorul, acesta fiind cel care a oferit cel mai apropiat răspuns de cel corect(în cazul în care toți jucătorii au oferit același răspuns castigator va fi cel care a răspuns primul)

Obs: Aplicația trebuie sa gestioneze cel puțin 100 de întrebări!

Fiecare jucător activ al jocului deține:
a. o regiune numită “bază” - cucerirea bazei unei jucator reprezinta pierderea tuturor regiunilor detinute de acesta

b. mai multe(sau niciuna) regiuni teritoriale

Jucătorul devine inactiv în momentul în care pierde regiunea “bază”

Fiecărei regiuni îi este atribuit un anumit scor, reprezentand importanța acesteia Jocul este alcatuit din 4 etape:

a. Etapa alegerii bazei: prin intermediul unei intrebari cu raspuns numeric va fi stabilită ordinea în care jucătorii își vor alege locul în care va fi construită bază următorului joc. Scorul atribuit acestei regiuni este 300

b. Etapa împărțirii regiunilor: prin intermediul unei intrebari cu raspuns numeric va fi stabilită ordinea și numărul de regiunii pe care le va putea selecta fiecare jucător(jucătorul de poziția k în clasament va putea selecta n-k regiuni care îi vor intra în posesie, n-fiind numărul de jucători). Această etapă se încheie în momentul în care toate regiunile din hartă sunt atribuite unui jucător, scorul acestora fiind 100 de unități.

c. Duelul: se va desfășura pe mai mai multe runde(numărul acestora diferă în funcție de numărul de jucători). În cadrul rundelor fiecare jucător(în ordine aleatoare) va încerca să cucerească o regiune vecină(2 regiuni sunt vecine doar dacă au cel puțin o granița comuna). Cei doi jucători se vor lupta iar dacă:

i. posesorul regiunii castiga lupta, atunci scorul regiunii va fi incrementat cu 100 de unități

ii. cel care a pornit duelul castiga, atunci regiunea intra în posesia acestuia cu scorul 100 doar dacă scopul acesteia era 100 inainte de duel, astfel regiunea va ramane posesia adversarului, dar scorul va fi decrementat cu 100 de unități

d. Stabilirea câștigătorilor: însumând scorurile tuturor regiunilor deținute de jucători după încheierea rundelor de joc

Avantaje: în cadrul luptelor, jucătorii pot aplica cel mult o data pe durata unui joc fiecare dintre următoarele avantaje:
a. 50-50: avantaj care va elimina 2 răspunsuri dintre cele 4 ale întrebărilor de tip grila

b. alegere răspuns: avantaj care oferă utilizatorului 4 răspunsuri din care poate alege în cazul întrebărilor cu răspuns numeric

c. sugerare răspuns: avantaj care sugerează utilizatorului răspunsul corect(sau o valoare apropiata cu acesta) în cazul întrebărilor cu răspuns numeric

Costul fiecărui avantaj este egal cu 100 de unități. Acest cost va fi plătit din scorul unei regiuni a cărei valoare este de minimum 200 de unități, regiunea care va plăti prețul va fi aleasă de jucător după încheierea luptei în care a folosit avantajul.

CERINTE DE BAZA:

➡ Retelistica: implementarea aplicației respectand arhitectura client-server(aplicația trebuie sa asigure posibilitatea creării a minim 2 instanțe de client + 1 aplicație server care vor comunica prin rețea).

➡ Pagină de Login/Register: la pornire, unui utilizator i se oferă posibilitatea de a se loga în contul său sau își poate crea un cont. Logarea/înregistrarea presupune introducerea numelui de utilizator. Atenție: numele de utilizator trebuie să fie unic!

➡ Pagină/Fereastra jocului: este fereastra în care va fi desenată harta(în varianta basic va fi afișată în consolă orice reprezentare a acesteia) și desfășurarea jocului.

Dimensiunea hartii este dependentă de numărul de jucători: 2 jucatori → harta de 3x3, 5 runde 3 jucatori → harta de 5x3, 4 runde 4 jucatori → harta de 6x4, 5 runde

Dimensiunea tablei de joc și numărul de runde pot fi modificate! A se urmări o implementare cat mai generică. Deasemenea, dimensiunea tablei de joc poate avea și altă formă(diferită de cea dreptunghiulară)

➡ Pagina de profil a utilizatorului: Aici un utilizator poate vizualiza un istoric al meciurilor jucate

COMPONENTE AVANSATE ALE PROIECTULUI:

➡ GUI - Să se implementeze o interfață grafică pentru cerințele de bază folosind framework-ul Qt.

➡ Bază de date: Pentru a vă organiza datele, puteți să le stocați într-o bază de date. Se va folosi biblioteca de SQLite SQLite ORM (NU ALTA). Se poate instala într-un proiect de Visual Studio folosind Microsoft vcpkg.

ELEMENTE DE MODERN C++:

➡ const ref ➡ move semantics ➡ structuri de date moderne - Containers library ➡ algoritmi moderni - Algorithms library ➡ smart pointers - Dynamic memory management ➡ template ➡ Regex ➡ DLL ➡ Studierea profilului si performantei aplicatiei


