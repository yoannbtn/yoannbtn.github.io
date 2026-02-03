## Portfolio

Click on the [*project name/image*] to display it.

---

[7]: /page/fabric-poc-strava-analytics

### [Strava Data Analytics Pipeline (Microsoft Fabric)][7]
[<img src="images/fabric_poc_wallpaper.png?raw=true"/>][7]
<div style="text-align:center; font-size:11px; margin-bottom: 2em;"> &copy; Yoann Betton / Fabric POC 2026 </div>

**Description:** End-to-end sports data analytics platform (Trail/Commuting) automated on Microsoft Fabric using Medallion Architecture and Direct Lake technology.

**Overview:**

This project implements a modern data infrastructure to monitor athletic preparation and gear wear. The pipeline automates data extraction from the Strava API into a Fabric Lakehouse. The architecture follows industry standards:
- **Bronze:** Raw ingestion of activities and gear snapshots (JSON files).
- **Silver:** Cleaning, explicit typing, and historical tracking (SCD) via PySpark.
- **Gold:** Star Schema optimized for high-performance reporting.

The project leverages the latest Power BI innovations with **Direct Lake** mode, enabling real-time querying of large Delta tables without import latency, all optimized for a Fabric F2 capacity.

**Tech Stack:**
- **Cloud & Orchestration:** Microsoft Fabric (Data Pipelines).
- **Processing:** PySpark (Spark 3.4 Notebooks).
- **Storage:** OneLake / Delta Lake (V3).
- **Visualization:** Power BI (Semantic Model & Direct Lake).

**Sources of data:**
- [Strava API](https://developers.strava.com/)

---


[6]: /page/wec-analysis

### [WEC Data Analysis Tool][6]
[<img src="images/wec_wallpaper.jpg?raw=true"/>][6]
<div style="text-align:center; font-size:11px; margin-bottom: 2em;"> &copy; Nico Deumille / 24h du Mans 2023 </div>


**Description:** Project dedicated to the analysis of WEC races, focusing on prominent events such as 24 Hours of Le Mans.

**Overview:**

Dive into a data-driven exploration of WEC endurance races, with a spotlight on the iconic 24 Hours of Le Mans. This project leverages APIs to collect real-time and historical race data, processes it using Power Query, and delivers dynamic visualizations through Power BI. The goal is to uncover performance trends, stint strategies, and team comparisons across race classes, offering motorsport enthusiasts and analysts a comprehensive view of endurance racing dynamics.

**Sources of data:**
  - [FIA WEC - Timing results](https://fiawec.alkamelsystems.com/)


---


[5]: /page/sailing-races-analysis

### [Sailing Races Analysis][5]
[<img src="images/ultim_wallpaper.JPG?raw=true"/>][5]
<div style="text-align:center; font-size:11px; margin-bottom: 2em;"> &copy; Alexis Courcoux / Route du Rhum 2022 </div>


**Description:** Project dedicated to the analysis of sailing races, focusing on prominent events such as La Transat Jacques Vabres, Route du Rhum, Arkéa Ultim Challenge, Trophée Jules Vernes, etc. 

**Overview:**

Explore Python scripts designed for scraping data from official race websites, trackers, and leaderboards. The goal is to facilitate the retrieval of sailing race data, offering performance analyses in the form of polars or heatmaps. Additionally, the repository provides datasets for further analysis using Python or Power BI.

**Sources of data:**
  - [Arkéa Ultim Challenge - Brest](https://arkeaultimchallengebrest.com/fr/classement)
  - [Transat Jacques Vabre Normandie - Le Havre](https://www.transatjacquesvabre.org/classement)
  - [Open-Meteo API](https://open-meteo.com/)


---


[4]: /page/imoca-sails


### [Comprendre les choix de voiles des IMOCA][4]
[<img src="images/imoca_wallpaper.jpg?raw=true"/>][4]
<div style="text-align:center; font-size:11px;"> &copy; Sailing Energy / The Ocean Race </div>


---


[1]: /page/f1-2023
[2]: https://github.com/yoannbtn/
[3]: https://gitlab.utc.fr/bettonyo

### [F1 Data Analysis Tool][1]
[<img src="output/2023-03-05_Bahrain_Grand_Prix/global_racepace_white.svg?raw=true"/>][1]

**Description:** Development of tools for analyzing and visualizing race data from the 2022 Formula One World Championship.

**Sources of data:**
  - [Formula one's live timing service](https://www.formula1.com/en/f1-live-timing.html)
  - [Ergast Developer API](https://ergast.com/mrd/)

**Python packages used:**
  - [`FastF1`](https://github.com/theOehrly/Fast-F1)
  - [`NumPy`](https://numpy.org/), [`Pandas`](https://pandas.pydata.org/)
  - [`Matplotlib`](https://matplotlib.org/), [`Seaborn`](https://seaborn.pydata.org/)

**Objectives:**

This personal project "F1 Data Analysis Tool" I am working on is about producing graphical visualizations to analyze the different sessions that take place within Grand Prix weekends of the 2022 FIA Formula 1 World Championship.

The idea is to develop a set of scripts to automatically collect, process, analyze and visualize race data through different forms of charts in order to extract knowledge.

The data used comes from the official Formula 1 API / live timing service.

**Motivations:**

This work has a double interest for me.

First of all, as a motorsport fan, I realize how complex this sport can be to understand. It is very difficult to figure out the different technical choices made by the teams as a TV viewer. It therefore seems to me to be an excellent opportunity to use this chronometric and telemetric data to try to understand, *a posteriori*, which technical directions have been picked.

In addition, this project allows me to directly apply the knowledge that I have learned and am learning as part of my studies in data science. It allows me to strengthen my practical skills.

**Time spent:**

The idea for this project came to me at the end of August 2022 when I started my exchange at Polytechnique Montréal. Students are especially encouraged to apply what they are studying there in any way.

I therefore went into this project by investing myself very strongly in it at the beginning: during the month of September, I consider that I spent about 2 hours to it per day, or 10 hours per week. Phases of reflection, design and development of scripts followed before getting the first graphics.

At the same time, I had to think about how I was going to show my results. I decided to create a section on my GitHub portfolio where I summarize all the analyzes I do. To make the site operational and to implement the automatic analysis generation procedure, it took me about twenty hours of work.
The small site is now operational and I ran the scripts on the first Grand Prix of this 2022 F1 season, the results are visible there.

The operational part for each new practice session, qualifying or race takes me about 15-20 minutes to generate the analyzes and check that the results are automatically included on the site.

Finally, I still have a lot of ideas for new analyzes in mind that I would like to develop. However, I also have to work on the academic projects I have this semester. This is why I am spending less time on this project now.

**How interested people can use it:**

These visualizations are intended for anyone who is a fan of Formula 1 looking to learn more about the performance of F1 racing cars and the different technical choices made by the teams (which represents, I am aware, a tiny part of the audience of this sport). They are publicly visible in the section of my portfolio dedicated to this project.

I am still in the development phase of this tool and would like to develop it more. In the future, I might consider releasing the scripts to generate these graphs.


**Future prospects:**

It may be that, in the future, I will collaborate with a sports journalist friend. I will take care of the creation of analyzes and exploitation of data, and he will focus on editorialization and presentation of the results. Nothing has been decided yet, but it is an eventuality that we are considering for the future in the medium term.


---

### Other projects

  - [GitHub][2]
  - [GitLab][3]

---

<div style="text-align: center">
  <p style="font-size:11px">&copy; 2026 Yoann Betton</p>
</div>

<!-- ---

<p style="font-size:11px">Page generated from <a href="https://github.com/yoannbtn/yoannbtn.github.io">github.com/yoannbtn</a>.</p> -->
