# analyza-futbalovych-zapasov
## Úvod

V našej práci sme sa zamerali na analýzu dát, ktoré obashujú informácie o zápasoch hraných v 18-tich európskych ligách. Naše dáta pocádzajú z webstránky [Kaggle](https://www.kaggle.com/datasets/bastekforever/complete-football-data-89000-matches-18-leagues/data)
 .

Otázky, ktoré sme si pri analýze kládli:

* Ako medzi sebou súvisia niektoré parametre zápasu (napr. počet strelených gólov domácich vs počet ofenzívnych akcií domácich) ?
* Aké predikčné modely, dokážu lepšie predikovať výsledky futbalových zápasov?
* Ktoré parametre sú najdôležitejšie pri predikovaní výsledku zápasu?
* Vieme iba na základe parametrov zápasu určiť kvalitu ligy?
* Sú predzápasové kurzy dobrým indikátorom výhry tímu?
* Ktoré tímy sú si výkonmi v zápasoch podobné?
* Má tím, ktorý vyhráva po prvých 15 minútach signifikantne vyššiu šancu na výhru?
* Ktoré tímy majú ofenzívny, obranný alebo vyvážený štýl hry?
* Majú tímy lepšie parametre hry, ak hrajú na domácom ihrisku?
* Ktoré tímy nehrajú rovnako dobre počas všetkých období, ale ich úspešnosť závisí od roku?


## Premenné vyskytujúce nachádzajúce sa v našom dátovom súbore
Spolu je 96 tisíc riadkov a 56 stĺpcov. Veľa riadkov obsahuje chýbajúce dáta

| **#** | **Parameter**            | **Popis**                                                                                   |
|-------|--------------------------|---------------------------------------------------------------------------------------------|
| 1     | **League**               | Súťaž alebo šampionát, ktorého je zápas súčasťou (napr. Premier League, La Liga).           |
| 2     | **Home**                 | Názov tímu, ktorý hrá na svojom štadióne/poli.                                              |
| 3     | **Away**                 | Názov tímu, ktorý hosťuje/súperí.                                                           |
| 4     | **INC**                  | Interný kód alebo indikátor súvisiaci so zápasom (napr. stav zápasu alebo jedinečné ID).    |
| 5     | **Round**                | Konkrétna fáza alebo „hrací deň“ v sezóne/súťaži (napr. 5. kolo).                          |
| 6     | **Date**                 | Kalendárny dátum, kedy sa zápas odohral.                                                   |
| 7     | **Time**                 | Čas začiatku zápasu.                                                                       |
| 8     | **H_Score**              | Celkový počet gólov, ktoré strelil domáci tím na konci zápasu (konečné skóre).             |
| 9     | **A_Score**              | Celkový počet gólov, ktoré strelil hosťujúci tím na konci zápasu (konečné skóre).          |
| 10    | **HT_H_Score**           | Počet gólov, ktoré strelil domáci tím do polčasu.                                          |
| 11    | **HT_A_Score**           | Počet gólov, ktoré strelil hosťujúci tím do polčasu.                                       |
| 12    | **WIN**                  | Kto vyhral zápas („Home“ = 1, „Away“ = -1, „Draw“ = 0).                                    |
| 13    | **H_BET**                | Kurzy na výhru domáceho tímu pred začiatkom zápasu.                                        |
| 14    | **X_BET**                | Kurzy na remízu pred začiatkom zápasu.                                                    |
| 15    | **A_BET**                | Kurzy na výhru hosťujúceho tímu pred začiatkom zápasu.                                     |
| 16    | **WIN_BET**              | Výsledok zápasu v stávkových podmienkach (ktorý kurz „vyhral“).                            |
| 17    | **OVER_2.5**             | Indikuje, či celkový počet gólov v zápase prekročil 2,5 (áno/nie).                         |
| 18    | **OVER_3.5**             | Indikuje, či celkový počet gólov v zápase prekročil 3,5 (áno/nie).                         |
| 19    | **H_15**                 | Či domáci tím viedol v 15. minúte (pravda/nepravda).                                       |
| 20    | **A_15**                 | Či hosťujúci tím viedol v 15. minúte (pravda/nepravda).                                    |
| 21    | **H_45_50**              | Či domáci tím viedol okolo 45.–50. minúty (pravda/nepravda).                               |
| 22    | **A_45_50**              | Či hosťujúci tím viedol okolo 45.–50. minúty (pravda/nepravda).                            |
| 23    | **H_90**                 | Či domáci tím viedol v 90. minúte (na konci zápasu) (pravda/nepravda).                     |
| 24    | **A_90**                 | Či hosťujúci tím viedol v 90. minúte (na konci zápasu) (pravda/nepravda).                  |
| 25    | **H_Missing_Players**    | Počet hráčov domáceho tímu, ktorí nemohli hrať.                                            |
| 26    | **A_Missing_Players**    | Počet hráčov hosťujúceho tímu, ktorí nemohli hrať.                                         |
| 27    | **Missing_Players**      | Celkový počet chýbajúcich hráčov (kombinácia domáceho a hosťujúceho tímu).                 |
| 28    | **H_Ball_Possession**    | Percento času, počas ktorého mal domáci tím kontrolu nad loptou.                           |
| 29    | **A_Ball_Possession**    | Percento času, počas ktorého mal hosťujúci tím kontrolu nad loptou.                        |
| 30    | **H_Goal_Attempts**      | Celkový počet pokusov (striel) domácim tímom na skórovanie.                                |
| 31    | **A_Goal_Attempts**      | Celkový počet pokusov (striel) hosťujúcim tímom na skórovanie.                             |
| 32    | **H_Shots_on_Goal**      | Počet striel domácim tímom, ktoré smerovali na bránu.                                      |
| 33    | **A_Shots_on_Goal**      | Počet striel hosťujúcim tímom, ktoré smerovali na bránu.                                   |
| 34    | **H_Attacks**            | Počet ofenzívnych akcií domáceho tímu považovaných za „útoky“.                             |
| 35    | **A_Attacks**            | Počet ofenzívnych akcií hosťujúceho tímu považovaných za „útoky“.                          |
| 36    | **H_Dangerous_Attacks**  | Útoky domáceho tímu, ktoré mali vysokú šancu na gól.                                       |
| 37    | **A_Dangerous_Attacks**  | Útoky hosťujúceho tímu, ktoré mali vysokú šancu na gól.                                    |
| 38    | **H_Shots_off_Goal**     | Strely domáceho tímu, ktoré úplne minuli bránu.                                            |
| 39    | **A_Shots_off_Goal**     | Strely hosťujúceho tímu, ktoré úplne minuli bránu.                                         |
| 40    | **H_Blocked_Shots**      | Strely domáceho tímu zablokované obrancami pred dosiahnutím brány.                         |
| 41    | **A_Blocked_Shots**      | Strely hosťujúceho tímu zablokované obrancami.                                             |
| 42    | **H_Free_Kicks**         | Počet voľných kopov pridelených domácemu tímu.                                             |
| 43    | **A_Free_Kicks**         | Počet voľných kopov pridelených hosťujúcemu tímu.                                          |
| 44    | **H_Corner_Kicks**       | Počet rohových kopov pridelených domácemu tímu.                                            |
| 45    | **A_Corner_Kicks**       | Počet rohových kopov pridelených hosťujúcemu tímu.                                         |
| 46    | **H_Offsides**           | Počet ofsajdov domáceho tímu.                                                              |
| 47    | **A_Offsides**           | Počet ofsajdov hosťujúceho tímu.                                                           |
| 48    | **H_Throw_in**           | Celkový počet vhadzovaní vykonaných domácim tímom.                                         |
| 49    | **A_Throw_in**           | Celkový počet vhadzovaní vykonaných hosťujúcim tímom.                                      |
| 50    | **H_Goalkeeper_Saves**   | Počet záchran brankára domáceho tímu.                                                     |
| 51    | **A_Goalkeeper_Saves**   | Počet záchran brankára hosťujúceho tímu.                                                  |
| 52    | **H_Fouls**              | Počet faulov, ktorých sa dopustil domáci tím.                                              |
| 53    | **A_Fouls**              | Počet faulov, ktorých sa dopustil hosťujúci tím.                                           |
| 54    | **H_Yellow_Cards**       | Počet žltých kariet udelených hráčom domáceho tímu.                                        |
| 55    | **A_Yellow_Cards**       | Počet žltých kariet udelených hráčom hosťujúceho tímu.                                     |
| 56    | **Game Link**            | Hyperlink alebo odkaz na podrobné informácie o zápase.                                     |

## EXPLORATIVNA ANALYZA

<p align="center">
  <img src="images/boxAttacks.png" alt="Image 1" width="250" />
  <img src="images/histHomeAttacks.png" alt="Image 2" width="250" />
  <img src="images/histAwayAttacks.png" alt="Image 3" width="250" />
</p>

<p>
 <img src="images/boxDangerous.png" alt="Image 1" width="250" />
  <img src="images/histHomeDangerous.png" alt="Image 2" width="250" />
  <img src="images/histAwayDangerous.png" alt="Image 3" width="250" />
</p>
