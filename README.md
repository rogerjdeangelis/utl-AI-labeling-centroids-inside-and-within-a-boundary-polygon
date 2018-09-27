# utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon
AI Labeling centroids inside and within a boundary polygon. 
    AI Labeling centroids inside and within a boundary polygon

    This may seem trivial but centroids are critical fo stability, spacecraft
    durability.... Artificial Inteligence.

    Centroid inside polygon
    -----------------------
    https://tinyurl.com/yajawf4h
    https://github.com/rogerjdeangelis/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon/blob/master/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon-inside.pdf


    Centroid within polygon
    -----------------------
    https://tinyurl.com/y85bmk72
    https://github.com/rogerjdeangelis/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon/blob/master/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon-outside.pdf

    github
    https://tinyurl.com/y955c9dm
    https://github.com/rogerjdeangelis/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon

    Mitch Profile
    https://stackoverflow.com/users/3609772/mitch

    Stackoverflow R
    https://tinyurl.com/y7t5s2u6
    https://stackoverflow.com/questions/52522872/r-sf-package-centroid-within-polygon

    INPUT
    =====

     SD1.HAVE total obs=14

       X      Y

      144    655
      115    666
       97    660
       86    640
       83    610
       97    583
      154    578
      140    560
       72    566
       59    600
       65    634
       86    678
      145    678
      144    655


         X
     700 +
         |
         |
         |     *-*---------- *
         |     |             |
         |     |     *-------*
         |     |    /
     650 +     |   /
         |     |  *
         |     |  |
     890 |     |  |
         |     |  |
         |     |  |
         |     |  *
     600 +     |   \
         |     |    \
         |     |     *-----------*
         |     |                /
         |     |               /
         |     |              /
         |     *------------ *
     550 +
         -+---------+---------+---------+ Y
         50        100       150      200


    EXAMPLE OUTPUT
    --------------


             Within Centroid
     700 +   ===============
         |
         |
         |     *-*---------- *
         |     |             |
         |     |     *-------*
         |     |    /
     650 +     |   /
         |     |  *
         |     |  |
     890 |     |  |
         |     |  |
         |     |  |   X  Centroid
         |     |  *      Outside
     600 +     |   \     Polygon
         |     |    \
         |     |     *-----------*
         |     |                /
         |     |               /
         |     |              /
         |     *------------ *
     550 +
         -+---------+---------+---------+
         50        100       150      200

                         X

     700 +    Inside Centroid
         |    ===============
         |
         |     *-*---------- *
         |     |             |
         |     |     *-------*
         |     |    /
     650 +     |   /
         |     |  *
         |     |  |
     890 |     |  |
         |     |  |
         |     |  |
         |     |  *
     600 +     |   \
         |     |  X \
         |     |     *-----------*
         |     |  Centroid      /
         |     |  Inside       /
         |     |  Polygon     /
         |     *------------ *
     550 +
         -+---------+---------+---------+
         50        100       150      200

                         X


    PROCESS
    =======

    OUTSIDE
    -------

    %utl_submit_r64('
    rm(list = ls(all = TRUE));
    library(sf);
    library(tidyverse);
    library(ggrepel);
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    pol <- st_polygon(list(as.matrix(have))) %>% st_sfc();
    a = data.frame(NAME = "X");
    st_geometry(a) = pol;
    pdf("d:/pdf/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon-outside.pdf");
    a <- a  %>%
      mutate(lon = map_dbl(geometry, ~st_centroid(.x)[[1]]),
         lat = map_dbl(geometry, ~st_centroid(.x)[[2]]));
    ggplot() +
      geom_sf(data = a, fill = "orange") +
      geom_label_repel(data = a, aes(x = lon, y = lat, label = NAME));
    pdf();
    ');


    INSIDE
    ------

    %utl_submit_r64('
    rm(list = ls(all = TRUE));
    library(sf);
    library(tidyverse);
    library(ggrepel);
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    pol <- st_polygon(list(as.matrix(have))) %>% st_sfc();
    a = data.frame(NAME = "A");
    st_geometry(a) = pol;
    pdf("d:/pdf/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon-inside.pdf");
    a2 <- a  %>%
      mutate(lon = map_dbl(geometry, ~st_point_on_surface(.x)[[1]]),
             lat = map_dbl(geometry, ~st_point_on_surface(.x)[[2]]));
    ggplot() +
      ggplot2::geom_sf(data = a2, fill = "orange") +
      geom_label_repel(data = a2, aes(x = lon, y = lat, label = NAME));
    pdf();
    ');


    OUTPUT
    ======

    https://tinyurl.com/yajawf4h
    https://tinyurl.com/y85bmk72


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;


    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input x  y;
    cards4;
    144 655
    115 666
     97 660
     86 640
     83 610
     97 583
    154 578
    140 560
     72 566
     59 600
     65 634
     86 678
    145 678
    144 655
    ;;;;
    run;quit;

