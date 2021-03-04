---
title: 'Weekly Exercises #5'
author: "Rohit Shah, ***WORKED WITH NOLAN MEYER AND OMAR ANWAR***"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---




```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(babynames)
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.


```r
lettuceQuantity <- 
  garden_harvest %>%
  filter (vegetable == "lettuce") %>%
  group_by(variety) %>%
  summarize(n = n()) %>%
  ggplot(aes(y = fct_reorder(variety, n), 
             x = n,
             text = variety)) +
  geom_col() +
  labs(title = "Quantity of Lettuce Variety Harvested", y = "Variety", x = "Count")

ggplotly(lettuceQuantity, tooltip = c("text","x"))
```

```{=html}
<div id="htmlwidget-d422b07b2daa115be206" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-d422b07b2daa115be206">{"x":{"data":[{"orientation":"v","width":[27,29,1,3,9],"base":[3.55,4.55,0.55,1.55,2.55],"x":[13.5,14.5,0.5,1.5,4.5],"y":[0.9,0.9,0.9,0.9,0.9],"text":["n: 27<br />Farmer's Market Blend","n: 29<br />Lettuce Mixture","n:  1<br />mustard greens","n:  3<br />reseed","n:  9<br />Tatsoi"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":148.310502283105},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Quantity of Lettuce Variety Harvested","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.45,30.45],"tickmode":"array","ticktext":["0","10","20","30"],"tickvals":[0,10,20,30],"categoryorder":"array","categoryarray":["0","10","20","30"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,5.6],"tickmode":"array","ticktext":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"tickvals":[1,2,3,4,5],"categoryorder":"array","categoryarray":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Variety","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"2f8644f42fcc":{"x":{},"y":{},"text":{},"type":"bar"}},"cur_data":"2f8644f42fcc","visdat":{"2f8644f42fcc":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  

```r
newBabyNames <-
  babynames %>%
  mutate(babynames, hasNames = n > 2000) %>%
  group_by(year, sex) %>%
  summarize(proportion = mean(hasNames)) %>%
  ggplot(aes(x = year, y = proportion, color = sex)) +
  geom_line() 

ggplotly(newBabyNames, tooltip = c("proportion", "year"))
```

```{=html}
<div id="htmlwidget-01df6982abd0bdb4de05" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-01df6982abd0bdb4de05">{"x":{"data":[{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.00318471337579618,0.00319829424307036,0.00486381322957198,0.0047438330170778,0.00511945392491468,0.0050125313283208,0.0062402496099844,0.00535987748851455,0.0101763907734057,0.0101419878296146,0.0110821382007823,0.0110893672537508,0.0132450331125828,0.0139225181598063,0.0135135135135135,0.0138274336283186,0.0142465753424658,0.0138966092273485,0.0156962025316456,0.0141150922909881,0.0179856115107914,0.013381369016984,0.0146914789422135,0.0134421507441191,0.0138568129330254,0.0147717099373321,0.0157657657657658,0.0154230929553981,0.0160230073952342,0.0160910518053375,0.0175627240143369,0.0174337517433752,0.0194484760522496,0.0194227137847316,0.0209224916785544,0.0203342057580028,0.0207283998450213,0.0210843373493976,0.0214822771213749,0.0215866162978953,0.021856027753686,0.0221427354794754,0.0222836413888409,0.0230005227391532,0.0233937955585692,0.0232195460058915,0.0234833659491194,0.0228449045154382,0.0224429727740986,0.0233175355450237,0.0232469512195122,0.0237090616837452,0.023921568627451,0.0236722931247427,0.0229237884576714,0.0237121831561733,0.0238879736408567,0.023949665110615,0.0244293151782139,0.0248384491114701,0.0250746268656716,0.0251720747295969,0.024907063197026,0.025521609538003,0.0247855100095329,0.0236596069452395,0.0237425255012311,0.0239226609864001,0.023841059602649,0.0240725474031327,0.0242186221567665,0.0249557237159878,0.0251916757940854,0.0252346514848438,0.0261487303506651,0.0258736059479554,0.0258533042846768,0.025812892184826,0.025918541726004,0.0252918287937743,0.0257809302960033,0.0251029353167751,0.0243966767770012,0.0246639697246509,0.023836985774702,0.0233638656577725,0.0221901260504202,0.0211491909797426,0.0198926043446424,0.0186035829122646,0.0176470588235294,0.0162896866569828,0.0148017803540006,0.0143804181540031,0.0137708760621154,0.0129135639551324,0.0127522935779817,0.0127163546450018,0.0118570183086312,0.0116152753405198,0.0118450275561405,0.0115706548498277,0.0120061653281415,0.0119353501864898,0.0119957275490921,0.01176,0.0110738516727755,0.0104111655978876,0.0103048209267133,0.0100371236078647,0.00984574991795208,0.00970308558121483,0.0098648388956505,0.00974868645945433,0.0100945971684337,0.0100933155589412,0.00987980617959851,0.00965346534653465,0.0095192191830341,0.00938551443244201,0.00940350082139013,0.00895937673900946,0.0089043747580333,0.0090613130765057,0.00897694677573569,0.00870607861536857,0.0086783042394015,0.00851167315175097,0.00811458180573887,0.00778036572674563,0.00767250517389329,0.00792433537832311,0.00810339522002257,0.00831990016119807,0.00849799280538032,0.00838838209080424,0.00818408885582186,0.00797422032880004],"text":["year: 1880<br />proportion: 0.003184713","year: 1881<br />proportion: 0.003198294","year: 1882<br />proportion: 0.004863813","year: 1883<br />proportion: 0.004743833","year: 1884<br />proportion: 0.005119454","year: 1885<br />proportion: 0.005012531","year: 1886<br />proportion: 0.006240250","year: 1887<br />proportion: 0.005359877","year: 1888<br />proportion: 0.010176391","year: 1889<br />proportion: 0.010141988","year: 1890<br />proportion: 0.011082138","year: 1891<br />proportion: 0.011089367","year: 1892<br />proportion: 0.013245033","year: 1893<br />proportion: 0.013922518","year: 1894<br />proportion: 0.013513514","year: 1895<br />proportion: 0.013827434","year: 1896<br />proportion: 0.014246575","year: 1897<br />proportion: 0.013896609","year: 1898<br />proportion: 0.015696203","year: 1899<br />proportion: 0.014115092","year: 1900<br />proportion: 0.017985612","year: 1901<br />proportion: 0.013381369","year: 1902<br />proportion: 0.014691479","year: 1903<br />proportion: 0.013442151","year: 1904<br />proportion: 0.013856813","year: 1905<br />proportion: 0.014771710","year: 1906<br />proportion: 0.015765766","year: 1907<br />proportion: 0.015423093","year: 1908<br />proportion: 0.016023007","year: 1909<br />proportion: 0.016091052","year: 1910<br />proportion: 0.017562724","year: 1911<br />proportion: 0.017433752","year: 1912<br />proportion: 0.019448476","year: 1913<br />proportion: 0.019422714","year: 1914<br />proportion: 0.020922492","year: 1915<br />proportion: 0.020334206","year: 1916<br />proportion: 0.020728400","year: 1917<br />proportion: 0.021084337","year: 1918<br />proportion: 0.021482277","year: 1919<br />proportion: 0.021586616","year: 1920<br />proportion: 0.021856028","year: 1921<br />proportion: 0.022142735","year: 1922<br />proportion: 0.022283641","year: 1923<br />proportion: 0.023000523","year: 1924<br />proportion: 0.023393796","year: 1925<br />proportion: 0.023219546","year: 1926<br />proportion: 0.023483366","year: 1927<br />proportion: 0.022844905","year: 1928<br />proportion: 0.022442973","year: 1929<br />proportion: 0.023317536","year: 1930<br />proportion: 0.023246951","year: 1931<br />proportion: 0.023709062","year: 1932<br />proportion: 0.023921569","year: 1933<br />proportion: 0.023672293","year: 1934<br />proportion: 0.022923788","year: 1935<br />proportion: 0.023712183","year: 1936<br />proportion: 0.023887974","year: 1937<br />proportion: 0.023949665","year: 1938<br />proportion: 0.024429315","year: 1939<br />proportion: 0.024838449","year: 1940<br />proportion: 0.025074627","year: 1941<br />proportion: 0.025172075","year: 1942<br />proportion: 0.024907063","year: 1943<br />proportion: 0.025521610","year: 1944<br />proportion: 0.024785510","year: 1945<br />proportion: 0.023659607","year: 1946<br />proportion: 0.023742526","year: 1947<br />proportion: 0.023922661","year: 1948<br />proportion: 0.023841060","year: 1949<br />proportion: 0.024072547","year: 1950<br />proportion: 0.024218622","year: 1951<br />proportion: 0.024955724","year: 1952<br />proportion: 0.025191676","year: 1953<br />proportion: 0.025234651","year: 1954<br />proportion: 0.026148730","year: 1955<br />proportion: 0.025873606","year: 1956<br />proportion: 0.025853304","year: 1957<br />proportion: 0.025812892","year: 1958<br />proportion: 0.025918542","year: 1959<br />proportion: 0.025291829","year: 1960<br />proportion: 0.025780930","year: 1961<br />proportion: 0.025102935","year: 1962<br />proportion: 0.024396677","year: 1963<br />proportion: 0.024663970","year: 1964<br />proportion: 0.023836986","year: 1965<br />proportion: 0.023363866","year: 1966<br />proportion: 0.022190126","year: 1967<br />proportion: 0.021149191","year: 1968<br />proportion: 0.019892604","year: 1969<br />proportion: 0.018603583","year: 1970<br />proportion: 0.017647059","year: 1971<br />proportion: 0.016289687","year: 1972<br />proportion: 0.014801780","year: 1973<br />proportion: 0.014380418","year: 1974<br />proportion: 0.013770876","year: 1975<br />proportion: 0.012913564","year: 1976<br />proportion: 0.012752294","year: 1977<br />proportion: 0.012716355","year: 1978<br />proportion: 0.011857018","year: 1979<br />proportion: 0.011615275","year: 1980<br />proportion: 0.011845028","year: 1981<br />proportion: 0.011570655","year: 1982<br />proportion: 0.012006165","year: 1983<br />proportion: 0.011935350","year: 1984<br />proportion: 0.011995728","year: 1985<br />proportion: 0.011760000","year: 1986<br />proportion: 0.011073852","year: 1987<br />proportion: 0.010411166","year: 1988<br />proportion: 0.010304821","year: 1989<br />proportion: 0.010037124","year: 1990<br />proportion: 0.009845750","year: 1991<br />proportion: 0.009703086","year: 1992<br />proportion: 0.009864839","year: 1993<br />proportion: 0.009748686","year: 1994<br />proportion: 0.010094597","year: 1995<br />proportion: 0.010093316","year: 1996<br />proportion: 0.009879806","year: 1997<br />proportion: 0.009653465","year: 1998<br />proportion: 0.009519219","year: 1999<br />proportion: 0.009385514","year: 2000<br />proportion: 0.009403501","year: 2001<br />proportion: 0.008959377","year: 2002<br />proportion: 0.008904375","year: 2003<br />proportion: 0.009061313","year: 2004<br />proportion: 0.008976947","year: 2005<br />proportion: 0.008706079","year: 2006<br />proportion: 0.008678304","year: 2007<br />proportion: 0.008511673","year: 2008<br />proportion: 0.008114582","year: 2009<br />proportion: 0.007780366","year: 2010<br />proportion: 0.007672505","year: 2011<br />proportion: 0.007924335","year: 2012<br />proportion: 0.008103395","year: 2013<br />proportion: 0.008319900","year: 2014<br />proportion: 0.008497993","year: 2015<br />proportion: 0.008388382","year: 2016<br />proportion: 0.008184089","year: 2017<br />proportion: 0.007974220"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"F","legendgroup":"F","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[0.0113421550094518,0.0120361083249749,0.010919017288444,0.0116504854368932,0.0106666666666667,0.0109389243391067,0.0108108108108108,0.0112464854732896,0.0110450297366185,0.0108010801080108,0.0103359173126615,0.00887311446317657,0.0103174603174603,0.0110262934690416,0.0104923325262308,0.0104754230459307,0.0102685624012638,0.00732302685109845,0.0100853374709077,0.00666666666666667,0.00929614873837981,0.00661157024793388,0.00833333333333333,0.00689127105666156,0.00788530465949821,0.00774102744546094,0.00778485491861288,0.00774693350548741,0.00757575757575758,0.00774270399047052,0.00815660685154975,0.0100050025012506,0.0147969717825189,0.0147194112235511,0.0159616919393456,0.0184510250569476,0.0183061314512572,0.0184702303346371,0.01849158528984,0.0180873180873181,0.0180360721442886,0.0180505415162455,0.0185222468290719,0.0185562805872757,0.0191146881287726,0.0193137456338607,0.0196402728964234,0.0199875078076202,0.0205334462320068,0.0206911732335461,0.0211407179035455,0.0210648148148148,0.0224089635854342,0.0223880597014925,0.0230568100784407,0.0231604342581423,0.0232846172900669,0.0243841751679522,0.0250247770069376,0.024955886059995,0.0261686991869919,0.02675,0.0289245982694685,0.028960396039604,0.0289076490150934,0.0296061326989162,0.0313510823587957,0.0328022492970947,0.032864967849488,0.0321199143468951,0.0319732760677643,0.0315219948247471,0.0338266384778013,0.0329569025120996,0.0333180147058824,0.0335004557885141,0.0352808988764045,0.0349297012302285,0.035341186930429,0.034769298053794,0.0359477124183007,0.0350386930352537,0.0346095608911962,0.0346395323663131,0.0350533420422382,0.034841628959276,0.0315255731922399,0.0312087912087912,0.0301624129930394,0.0297619047619048,0.0283609576427256,0.0259946949602122,0.0231304347826087,0.0219537100068074,0.0218151540383014,0.0203630623520126,0.020181790171006,0.0194132243468107,0.0192336144400059,0.0190920661858294,0.0189352360043908,0.0188031841888553,0.0191472026072787,0.0182611065685473,0.0185488270594654,0.0187310381216198,0.0175057500638896,0.0171821305841924,0.0167314716625427,0.0163650157147502,0.0162378743146352,0.0164835164835165,0.0161980440097799,0.0156357557281935,0.0156188988676298,0.0159775346179917,0.0159513862514242,0.015539728054759,0.0147774533227148,0.0150745111551383,0.0151039947177286,0.0150418733230344,0.0149815734657907,0.0147416294205285,0.0145234493192133,0.0144417838970368,0.0138255416191562,0.0138290479499653,0.0134811469239718,0.0131515527094953,0.0128367003367003,0.0131771595900439,0.0132078122804552,0.01410457330104,0.0143091051470065,0.0143325727324586,0.0141222991102952,0.0138418079096045],"text":["year: 1880<br />proportion: 0.011342155","year: 1881<br />proportion: 0.012036108","year: 1882<br />proportion: 0.010919017","year: 1883<br />proportion: 0.011650485","year: 1884<br />proportion: 0.010666667","year: 1885<br />proportion: 0.010938924","year: 1886<br />proportion: 0.010810811","year: 1887<br />proportion: 0.011246485","year: 1888<br />proportion: 0.011045030","year: 1889<br />proportion: 0.010801080","year: 1890<br />proportion: 0.010335917","year: 1891<br />proportion: 0.008873114","year: 1892<br />proportion: 0.010317460","year: 1893<br />proportion: 0.011026293","year: 1894<br />proportion: 0.010492333","year: 1895<br />proportion: 0.010475423","year: 1896<br />proportion: 0.010268562","year: 1897<br />proportion: 0.007323027","year: 1898<br />proportion: 0.010085337","year: 1899<br />proportion: 0.006666667","year: 1900<br />proportion: 0.009296149","year: 1901<br />proportion: 0.006611570","year: 1902<br />proportion: 0.008333333","year: 1903<br />proportion: 0.006891271","year: 1904<br />proportion: 0.007885305","year: 1905<br />proportion: 0.007741027","year: 1906<br />proportion: 0.007784855","year: 1907<br />proportion: 0.007746934","year: 1908<br />proportion: 0.007575758","year: 1909<br />proportion: 0.007742704","year: 1910<br />proportion: 0.008156607","year: 1911<br />proportion: 0.010005003","year: 1912<br />proportion: 0.014796972","year: 1913<br />proportion: 0.014719411","year: 1914<br />proportion: 0.015961692","year: 1915<br />proportion: 0.018451025","year: 1916<br />proportion: 0.018306131","year: 1917<br />proportion: 0.018470230","year: 1918<br />proportion: 0.018491585","year: 1919<br />proportion: 0.018087318","year: 1920<br />proportion: 0.018036072","year: 1921<br />proportion: 0.018050542","year: 1922<br />proportion: 0.018522247","year: 1923<br />proportion: 0.018556281","year: 1924<br />proportion: 0.019114688","year: 1925<br />proportion: 0.019313746","year: 1926<br />proportion: 0.019640273","year: 1927<br />proportion: 0.019987508","year: 1928<br />proportion: 0.020533446","year: 1929<br />proportion: 0.020691173","year: 1930<br />proportion: 0.021140718","year: 1931<br />proportion: 0.021064815","year: 1932<br />proportion: 0.022408964","year: 1933<br />proportion: 0.022388060","year: 1934<br />proportion: 0.023056810","year: 1935<br />proportion: 0.023160434","year: 1936<br />proportion: 0.023284617","year: 1937<br />proportion: 0.024384175","year: 1938<br />proportion: 0.025024777","year: 1939<br />proportion: 0.024955886","year: 1940<br />proportion: 0.026168699","year: 1941<br />proportion: 0.026750000","year: 1942<br />proportion: 0.028924598","year: 1943<br />proportion: 0.028960396","year: 1944<br />proportion: 0.028907649","year: 1945<br />proportion: 0.029606133","year: 1946<br />proportion: 0.031351082","year: 1947<br />proportion: 0.032802249","year: 1948<br />proportion: 0.032864968","year: 1949<br />proportion: 0.032119914","year: 1950<br />proportion: 0.031973276","year: 1951<br />proportion: 0.031521995","year: 1952<br />proportion: 0.033826638","year: 1953<br />proportion: 0.032956903","year: 1954<br />proportion: 0.033318015","year: 1955<br />proportion: 0.033500456","year: 1956<br />proportion: 0.035280899","year: 1957<br />proportion: 0.034929701","year: 1958<br />proportion: 0.035341187","year: 1959<br />proportion: 0.034769298","year: 1960<br />proportion: 0.035947712","year: 1961<br />proportion: 0.035038693","year: 1962<br />proportion: 0.034609561","year: 1963<br />proportion: 0.034639532","year: 1964<br />proportion: 0.035053342","year: 1965<br />proportion: 0.034841629","year: 1966<br />proportion: 0.031525573","year: 1967<br />proportion: 0.031208791","year: 1968<br />proportion: 0.030162413","year: 1969<br />proportion: 0.029761905","year: 1970<br />proportion: 0.028360958","year: 1971<br />proportion: 0.025994695","year: 1972<br />proportion: 0.023130435","year: 1973<br />proportion: 0.021953710","year: 1974<br />proportion: 0.021815154","year: 1975<br />proportion: 0.020363062","year: 1976<br />proportion: 0.020181790","year: 1977<br />proportion: 0.019413224","year: 1978<br />proportion: 0.019233614","year: 1979<br />proportion: 0.019092066","year: 1980<br />proportion: 0.018935236","year: 1981<br />proportion: 0.018803184","year: 1982<br />proportion: 0.019147203","year: 1983<br />proportion: 0.018261107","year: 1984<br />proportion: 0.018548827","year: 1985<br />proportion: 0.018731038","year: 1986<br />proportion: 0.017505750","year: 1987<br />proportion: 0.017182131","year: 1988<br />proportion: 0.016731472","year: 1989<br />proportion: 0.016365016","year: 1990<br />proportion: 0.016237874","year: 1991<br />proportion: 0.016483516","year: 1992<br />proportion: 0.016198044","year: 1993<br />proportion: 0.015635756","year: 1994<br />proportion: 0.015618899","year: 1995<br />proportion: 0.015977535","year: 1996<br />proportion: 0.015951386","year: 1997<br />proportion: 0.015539728","year: 1998<br />proportion: 0.014777453","year: 1999<br />proportion: 0.015074511","year: 2000<br />proportion: 0.015103995","year: 2001<br />proportion: 0.015041873","year: 2002<br />proportion: 0.014981573","year: 2003<br />proportion: 0.014741629","year: 2004<br />proportion: 0.014523449","year: 2005<br />proportion: 0.014441784","year: 2006<br />proportion: 0.013825542","year: 2007<br />proportion: 0.013829048","year: 2008<br />proportion: 0.013481147","year: 2009<br />proportion: 0.013151553","year: 2010<br />proportion: 0.012836700","year: 2011<br />proportion: 0.013177160","year: 2012<br />proportion: 0.013207812","year: 2013<br />proportion: 0.014104573","year: 2014<br />proportion: 0.014309105","year: 2015<br />proportion: 0.014332573","year: 2016<br />proportion: 0.014122299","year: 2017<br />proportion: 0.013841808"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,191,196,1)","dash":"solid"},"hoveron":"points","name":"M","legendgroup":"M","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.2283105022831,"r":7.30593607305936,"b":40.1826484018265,"l":48.9497716894977},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1873.15,2023.85],"tickmode":"array","ticktext":["1880","1920","1960","2000"],"tickvals":[1880,1920,1960,2000],"categoryorder":"array","categoryarray":["1880","1920","1960","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"year","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.00154656342367095,0.0375858623704259],"tickmode":"array","ticktext":["0.01","0.02","0.03"],"tickvals":[0.01,0.02,0.03],"categoryorder":"array","categoryarray":["0.01","0.02","0.03"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"proportion","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.913385826771654},"annotations":[{"text":"sex","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"2f86652994f9":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"2f86652994f9","visdat":{"2f86652994f9":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
delayedAIX <-
  small_trains %>%
  filter(departure_station == "AIX EN PROVENCE TGV") %>%
  group_by(month) %>%
  mutate(medianNumDelayed = median(delayed_number)) %>%
  ggplot(aes(x = month, y = medianNumDelayed)) +
  geom_point() +
  scale_color_viridis_d() +
  theme(axis.title.x = element_blank(), axis.title.y = element_blank()) +
  labs(title = "Median Number of Delayed Departures from Aix En Provence TGV, per Month") +
  transition_time(month)

animate(delayedAIX, duration = 30)
```

![](05_exercises_files/figure-html/unnamed-chunk-3-1.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>%
  mutate(cumulative_harvest = cumsum(daily_harvest_lb)) %>%
  ggplot() +
  geom_area(aes(x = date, y = cumulative_harvest, 
                fill = fct_reorder2(variety, variety, cumulative_harvest,.desc = FALSE))) +
  theme(legend.position = "top") +
  labs(title = "Cumulative Harvest of Tomatoes, by Variety (In lbs)",
       subtitle = "Date: {frame_along}") +
  transition_reveal(date)
```

![](05_exercises_files/figure-html/unnamed-chunk-4-1.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorcaMap <- get_stamenmap(
    bbox = c(left = 2.28, bottom = 39.41, right = 3.03, top = 39.8), 
    maptype = "terrain",
    zoom = 10)

ggmap(mallorcaMap) + 
  geom_path(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele), 
             size = .5) +
  geom_point(data = mallorca_bike_day7, aes(x = lon, y = lat), 
             size = 0.7, color = "red") +
  scale_color_viridis_c() +
  labs(subtitle = "Time: {frame_along}") +
  theme_map() +
  transition_reveal(time)
```

![](05_exercises_files/figure-html/unnamed-chunk-5-1.gif)<!-- -->
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panamTotal <- bind_rows(panama_swim, panama_bike, panama_run)

panamaMap <- get_stamenmap(
  bbox = c(left = -79.6140, bottom = 8.8701, top = 8.9875, right = -79.4270),
  maptype = "terrain",
  zoom = 12)

ggmap(panamaMap) +
  geom_path(data = panamTotal, 
            aes(x = lon, y = lat),
            size = 0.3) +
  geom_point(data = panamTotal,
             aes(x = lon, y = lat, color = event),
             size = 0.6) +
  labs(subtitle = "Time: {frame_along}") +
  theme_map() +
  transition_reveal(time)
```

![](05_exercises_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe. 
  

```r
myCovid19 <-
  covid19 %>%
  group_by(state) %>%
  arrange(state) %>%
  mutate(week_lag = lag(cases, 7)) %>%
  replace_na(list(week_lag = 0)) %>%
  mutate(new_cases_per_week = cases - week_lag) %>%
  replace_na(list(new_cases_per_week = 0)) %>%
  filter(cases > 20) %>%
  ggplot(aes(x = cases, y = new_cases_per_week, group = state)) +
  geom_path() +
  geom_text(aes(label = state)) +
  scale_x_log10() +
  scale_y_log10() +
  transition_reveal(date)

animate(myCovid19, duration = 30, nframes = 200)
```

![](05_exercises_files/figure-html/unnamed-chunk-7-1.gif)<!-- -->
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

## Parsed with column specification:
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
  
states_map <- map_data("state")
```


```r
case <- covid19 %>%
  mutate(state_name = str_to_lower(state)) %>%
  arrange(desc(date)) %>%
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>%
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000,
         day_of_week = wday(date, label = TRUE)) %>%
  filter(day_of_week == "Fri") %>%
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = cases_per_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  theme_map() +
  labs(title = "COVID-19 Cases per 10,000 People in Different States by Color",
       fill = "Cases per 10,000",
       subtitle = "Date: {frame_time}") +
  transition_time(date)

case

animate(case, nframes = 200, end_pause = 10)
```

![](05_exercises_files/figure-html/unnamed-chunk-9-1.gif)<!-- -->

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
