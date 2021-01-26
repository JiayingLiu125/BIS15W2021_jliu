---
title: "Lab 5 Homework"
author: "Jiaying Liu"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## i Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

```r
superhero_info <- rename(superhero_info, gender="Gender", eye_color="Eye color", race="Race", hair_color="Hair color", height="Height", publisher="Publisher", skin_color="Skin color", alignment="Alignment", weight="Weight")
superhero_info
```

```
## # A tibble: 734 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 A-Bo~ Male   yellow    Human No Hair       203 Marvel C~ <NA>       good     
##  2 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  3 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  4 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  5 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  6 Abso~ Male   blue      Human No Hair       193 Marvel C~ <NA>       bad      
##  7 Adam~ Male   blue      <NA>  Blond          NA NBC - He~ <NA>       good     
##  8 Adam~ Male   blue      Human Blond         185 DC Comics <NA>       good     
##  9 Agen~ Female blue      <NA>  Blond         173 Marvel C~ <NA>       good     
## 10 Agen~ Male   brown     Human Brown         178 Marvel C~ <NA>       good     
## # ... with 724 more rows, and 1 more variable: weight <dbl>
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He~ `Lantern Power ~ `Dimensional Aw~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati~ FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, ...
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral <- filter(superhero_info, alignment == "neutral")
neutral
```

```
## # A tibble: 24 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Biza~ Male   black     Biza~ Black         191 DC Comics white      neutral  
##  2 Blac~ Male   <NA>      God ~ <NA>           NA DC Comics <NA>       neutral  
##  3 Capt~ Male   brown     Human Brown          NA DC Comics <NA>       neutral  
##  4 Copy~ Female red       Muta~ White         183 Marvel C~ blue       neutral  
##  5 Dead~ Male   brown     Muta~ No Hair       188 Marvel C~ <NA>       neutral  
##  6 Deat~ Male   blue      Human White         193 DC Comics <NA>       neutral  
##  7 Etri~ Male   red       Demon No Hair       193 DC Comics yellow     neutral  
##  8 Gala~ Male   black     Cosm~ Black         876 Marvel C~ <NA>       neutral  
##  9 Glad~ Male   blue      Stro~ Blue          198 Marvel C~ purple     neutral  
## 10 Indi~ Female <NA>      Alien Purple         NA DC Comics <NA>       neutral  
## # ... with 14 more rows, and 1 more variable: weight <dbl>
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>%
  select(name, alignment, race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>%
  filter(race != "Human")
```

```
## # A tibble: 222 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Abe ~ Male   blue      Icth~ No Hair       191 Dark Hor~ blue       good     
##  2 Abin~ Male   blue      Unga~ No Hair       185 DC Comics red        good     
##  3 Abom~ Male   green     Huma~ No Hair       203 Marvel C~ <NA>       bad      
##  4 Abra~ Male   blue      Cosm~ Black          NA Marvel C~ <NA>       bad      
##  5 Ajax  Male   brown     Cybo~ Black         193 Marvel C~ <NA>       bad      
##  6 Alien Male   <NA>      Xeno~ No Hair       244 Dark Hor~ black      bad      
##  7 Amazo Male   red       Andr~ <NA>          257 DC Comics <NA>       bad      
##  8 Angel Male   <NA>      Vamp~ <NA>           NA Dark Hor~ <NA>       good     
##  9 Ange~ Female yellow    Muta~ Black         165 Marvel C~ <NA>       good     
## 10 Anti~ Male   yellow    God ~ No Hair        61 DC Comics <NA>       bad      
## # ... with 212 more rows, and 1 more variable: weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- data.frame(filter(superhero_info, alignment == "good"))
good_guys
```

```
##                          name gender               eye_color              race
## 1                      A-Bomb   Male                  yellow             Human
## 2                  Abe Sapien   Male                    blue     Icthyo Sapien
## 3                    Abin Sur   Male                    blue           Ungaran
## 4                 Adam Monroe   Male                    blue              <NA>
## 5                Adam Strange   Male                    blue             Human
## 6                    Agent 13 Female                    blue              <NA>
## 7                   Agent Bob   Male                   brown             Human
## 8                  Agent Zero   Male                    <NA>              <NA>
## 9                  Alan Scott   Male                    blue              <NA>
## 10               Alex Woolsly   Male                    <NA>              <NA>
## 11          Alfred Pennyworth   Male                    blue             Human
## 12           Allan Quatermain   Male                    <NA>              <NA>
## 13             Ando Masahashi   Male                    <NA>              <NA>
## 14                      Angel   Male                    blue              <NA>
## 15                      Angel   Male                    <NA>           Vampire
## 16                 Angel Dust Female                  yellow            Mutant
## 17            Angel Salvadore Female                   brown              <NA>
## 18                 Animal Man   Male                    blue             Human
## 19                    Ant-Man   Male                    blue             Human
## 20                 Ant-Man II   Male                    blue             Human
## 21                   Aquababy   Male                    blue              <NA>
## 22                    Aqualad   Male                    blue         Atlantean
## 23                    Aquaman   Male                    blue         Atlantean
## 24                    Arachne Female                    blue             Human
## 25                  Archangel   Male                    blue            Mutant
## 26                     Ardina Female                   white             Alien
## 27                       Ares   Male                   brown              <NA>
## 28                      Ariel Female                  purple              <NA>
## 29                      Armor Female                   black              <NA>
## 30                    Arsenal   Male                    <NA>             Human
## 31                  Astro Boy   Male                   brown              <NA>
## 32                      Atlas   Male                   brown            Mutant
## 33                       Atom   Male                    blue              <NA>
## 34                       Atom   Male                    <NA>              <NA>
## 35                  Atom Girl Female                   black              <NA>
## 36                    Atom II   Male                   brown             Human
## 37                   Atom III   Male                    <NA>              <NA>
## 38                    Atom IV   Male                   brown              <NA>
## 39                     Aurora Female                    blue            Mutant
## 40                     Azrael   Male                   brown             Human
## 41                      Aztar   Male                    <NA>              <NA>
## 42                    Banshee   Male                   green             Human
## 43                     Bantam   Male                   brown              <NA>
## 44                    Batgirl Female                    <NA>              <NA>
## 45                    Batgirl Female                   green             Human
## 46                Batgirl III Female                    <NA>              <NA>
## 47                 Batgirl IV Female                   green             Human
## 48                  Batgirl V Female                    <NA>              <NA>
## 49                 Batgirl VI Female                    blue              <NA>
## 50                     Batman   Male                    blue             Human
## 51                     Batman   Male                    blue             Human
## 52                  Batman II   Male                    blue             Human
## 53                 Battlestar   Male                   brown              <NA>
## 54                 Batwoman V Female                   green             Human
## 55                       Beak   Male                   black              <NA>
## 56                      Beast   Male                    blue            Mutant
## 57                  Beast Boy   Male                   green             Human
## 58                     Ben 10   Male                    <NA>              <NA>
## 59              Beta Ray Bill   Male                    <NA>              <NA>
## 60                   Beyonder   Male                    <NA>     God / Eternal
## 61                  Big Daddy   Male                    <NA>              <NA>
## 62                Bill Harken   Male                    <NA>             Alpha
## 63                     Binary Female                    blue              <NA>
## 64               Bionic Woman Female                    blue            Cyborg
## 65                 Bird-Brain   <NA>                    <NA>              <NA>
## 66                    Birdman   Male                    <NA>     God / Eternal
## 67                     Bishop   Male                   brown            Mutant
## 68                 Black Bolt   Male                    blue           Inhuman
## 69               Black Canary Female                    blue             Human
## 70               Black Canary Female                    blue         Metahuman
## 71                  Black Cat Female                   green             Human
## 72              Black Goliath   Male                    <NA>              <NA>
## 73           Black Knight III   Male                   brown             Human
## 74            Black Lightning   Male                   brown              <NA>
## 75              Black Panther   Male                   brown             Human
## 76                Black Widow Female                   green             Human
## 77             Black Widow II Female                    blue              <NA>
## 78                      Blade   Male                   brown           Vampire
## 79                Blaquesmith   <NA>                   black              <NA>
## 80                     Bling! Female                    <NA>              <NA>
## 81                      Blink Female                   green            Mutant
## 82                  Bloodhawk   Male                   black            Mutant
## 83                Blue Beetle   Male                    blue              <NA>
## 84                Blue Beetle   Male                    <NA>              <NA>
## 85             Blue Beetle II   Male                    blue              <NA>
## 86            Blue Beetle III   Male                   brown             Human
## 87                       Bolt   Male                    <NA>              <NA>
## 88                  Boom-Boom Female                    blue            Mutant
## 89                     Boomer Female                    <NA>              <NA>
## 90               Booster Gold   Male                    blue             Human
## 91                        Box   Male                    <NA>              <NA>
## 92                    Box III   <NA>                    blue              <NA>
## 93                     Box IV   <NA>                   brown              <NA>
## 94                 Brainiac 5   Male                   green              <NA>
## 95             Brother Voodoo   Male                   brown             Human
## 96                      Buffy Female                   green             Human
## 97                  Bumblebee Female                   brown             Human
## 98                  Bumbleboy   Male                    <NA>              <NA>
## 99                    Bushido   Male                    <NA>             Human
## 100                     Cable   Male                    blue            Mutant
## 101             Cameron Hicks   Male                    <NA>             Alpha
## 102                Cannonball   Male                    blue              <NA>
## 103           Captain America   Male                    blue             Human
## 104              Captain Atom   Male                    blue Human / Radiation
## 105           Captain Britain   Male                    blue             Human
## 106              Captain Epic   Male                    blue              <NA>
## 107         Captain Hindsight   Male                    <NA>             Human
## 108          Captain Mar-vell   Male                    blue              <NA>
## 109            Captain Marvel Female                    blue        Human-Kree
## 110            Captain Marvel   Male                    blue             Human
## 111         Captain Marvel II   Male                    blue             Human
## 112          Captain Midnight   Male                    <NA>             Human
## 113            Captain Planet   Male                     red     God / Eternal
## 114          Captain Universe   <NA>                    <NA>     God / Eternal
## 115                       Cat Female                    blue              <NA>
## 116                    Cat II Female                    <NA>              <NA>
## 117                  Catwoman Female                   green             Human
## 118             Cecilia Reyes   <NA>                   brown              <NA>
## 119                   Century   Male                   white             Alien
## 120                   Cerebra Female                    <NA>            Mutant
## 121                   Chamber   Male                   brown            Mutant
## 122              Chuck Norris   Male                    <NA>              <NA>
## 123             Citizen Steel   Male                   green             Human
## 124             Claire Bennet Female                    blue              <NA>
## 125                      Clea   <NA>                    <NA>              <NA>
## 126                     Cloak   Male                   brown              <NA>
## 127              Colin Wagner   Male                    grey              <NA>
## 128              Colossal Boy   Male                    <NA>              <NA>
## 129                  Colossus   Male                  silver            Mutant
## 130                   Corsair   Male                   brown              <NA>
## 131          Crimson Crusader   Male                    blue              <NA>
## 132            Crimson Dynamo   Male                   brown              <NA>
## 133                   Crystal Female                   green           Inhuman
## 134                    Cyborg   Male                   brown            Cyborg
## 135                   Cyclops   Male                   brown            Mutant
## 136                    Cypher   <NA>                    blue              <NA>
## 137                    Dagger Female                    blue              <NA>
## 138              Danny Cooper   Male                   brown              <NA>
## 139             Daphne Powell Female                    <NA>              <NA>
## 140                 Daredevil   Male                    blue             Human
## 141                  Darkhawk   Male                   brown             Human
## 142                   Darkman   Male                    <NA>              <NA>
## 143                  Darkstar Female                   brown            Mutant
## 144                      Dash   Male                    blue             Human
## 145                      Data   Male                  yellow           Android
## 146                   Dazzler Female                    blue            Mutant
## 147                   Deadman   Male                    blue             Human
## 148                  Deathlok   Male                   brown            Cyborg
## 149                DL Hawkins   Male                    <NA>              <NA>
## 150                Doc Samson   Male                    blue Human / Radiation
## 151               Doctor Fate   Male                    blue             Human
## 152            Doctor Strange   Male                    grey             Human
## 153                    Domino Female                    blue             Human
## 154                 Donatello   Male                   green            Mutant
## 155                Donna Troy Female                    blue            Amazon
## 156              Dr Manhattan   Male                   white    Human / Cosmic
## 157        Drax the Destroyer   Male                     red   Human / Altered
## 158                Elastigirl Female                   brown             Human
## 159                   Elektra Female                    blue             Human
## 160             Elongated Man   Male                    blue              <NA>
## 161                Emma Frost Female                    blue              <NA>
## 162               Enchantress Female                    blue             Human
## 163                    Energy Female                    <NA>              <NA>
## 164                     ERG-1   Male                    <NA>              <NA>
## 165                Ethan Hunt   Male                   brown             Human
## 166                    Falcon   Male                   brown             Human
## 167                     Feral   <NA> yellow (without irises)              <NA>
## 168           Fighting Spirit Female                    <NA>              <NA>
## 169             Fin Fang Foom   Male                     red   Kakarantharaian
## 170                  Firebird Female                   brown              <NA>
## 171                  Firelord   <NA>                   white              <NA>
## 172                  Firestar Female                   green            Mutant
## 173                 Firestorm   Male                   brown              <NA>
## 174                 Firestorm   Male                    blue             Human
## 175                     Flash   Male                    blue             Human
## 176              Flash Gordon   Male                    <NA>              <NA>
## 177                  Flash II   Male                    blue             Human
## 178                 Flash III   Male                    <NA>             Human
## 179                  Flash IV   Male                  yellow             Human
## 180                     Forge   <NA>                   brown              <NA>
## 181         Franklin Richards   Male                    blue            Mutant
## 182            Franklin Storm   <NA>                    blue              <NA>
## 183                    Frigga Female                    blue              <NA>
## 184                    Gambit   Male                     red            Mutant
## 185                    Gamora Female                  yellow     Zen-Whoberian
## 186               Garbage Man   Male                    <NA>            Mutant
## 187                 Gary Bell   Male                    <NA>             Alpha
## 188                   Genesis   Male                    blue              <NA>
## 189               Ghost Rider   Male                     red             Demon
## 190            Ghost Rider II   <NA>                    <NA>              <NA>
## 191                 Giant-Man   Male                    <NA>             Human
## 192              Giant-Man II   Male                    <NA>              <NA>
## 193                      Goku   Male                    <NA>            Saiyan
## 194                   Goliath   Male                    <NA>              <NA>
## 195                   Goliath   Male                    <NA>             Human
## 196                   Goliath   Male                    <NA>             Human
## 197                Goliath IV   Male                   brown              <NA>
## 198                   Gravity   Male                    blue             Human
## 199               Green Arrow   Male                   green             Human
## 200          Green Goblin III   Male                    <NA>              <NA>
## 201           Green Goblin IV   Male                   green              <NA>
## 202                     Groot   Male                  yellow    Flora Colossus
## 203                  Guardian   Male                   brown             Human
## 204               Guy Gardner   Male                    blue   Human-Vuldarian
## 205                Hal Jordan   Male                   brown             Human
## 206                  Han Solo   Male                   brown             Human
## 207                   Hancock   Male                   brown             Human
## 208              Harry Potter   Male                   green             Human
## 209                     Havok   Male                    blue            Mutant
## 210                      Hawk   Male                     red              <NA>
## 211                   Hawkeye   Male                    blue             Human
## 212                Hawkeye II Female                    blue             Human
## 213                  Hawkgirl Female                   green              <NA>
## 214                   Hawkman   Male                    blue              <NA>
## 215                 Hawkwoman Female                   green              <NA>
## 216              Hawkwoman II Female                    <NA>              <NA>
## 217             Hawkwoman III Female                    blue              <NA>
## 218                   Hellboy   Male                    gold             Demon
## 219                   Hellcat Female                    blue             Human
## 220                 Hellstorm   Male                     red              <NA>
## 221                  Hercules   Male                    blue          Demi-God
## 222             Hiro Nakamura   Male                    <NA>              <NA>
## 223                  Hit-Girl Female                    <NA>             Human
## 224                    Hollow Female                    blue              <NA>
## 225              Hope Summers Female                   green              <NA>
## 226           Howard the Duck   Male                   brown              <NA>
## 227                      Hulk   Male                   green Human / Radiation
## 228               Human Torch   Male                    blue Human / Radiation
## 229                  Huntress Female                    blue              <NA>
## 230                      Husk Female                    blue            Mutant
## 231                    Hybrid   Male                   brown          Symbiote
## 232                  Hyperion   Male                    blue           Eternal
## 233                    Iceman   Male                   brown            Mutant
## 234                   Impulse   Male                  yellow             Human
## 235             Indiana Jones   Male                    <NA>             Human
## 236                       Ink   Male                    blue            Mutant
## 237           Invisible Woman Female                    blue Human / Radiation
## 238                 Iron Fist   Male                    blue             Human
## 239                  Iron Man   Male                    blue             Human
## 240                      Isis Female                    <NA>              <NA>
## 241                Jack Bauer   Male                    <NA>              <NA>
## 242            Jack of Hearts   Male            blue / white             Human
## 243                 Jack-Jack   Male                    blue             Human
## 244                James Bond   Male                    blue             Human
## 245             James T. Kirk   Male                   hazel             Human
## 246             Jar Jar Binks   Male                  yellow            Gungan
## 247              Jason Bourne   Male                    <NA>             Human
## 248                 Jean Grey Female                   green            Mutant
## 249           Jean-Luc Picard   Male                    <NA>             Human
## 250             Jennifer Kale Female                    blue              <NA>
## 251               Jesse Quick Female                    <NA>             Human
## 252              Jessica Cruz Female                   green             Human
## 253             Jessica Jones Female                   brown             Human
## 254           Jessica Sanders Female                    <NA>              <NA>
## 255                Jim Powell   Male                    <NA>              <NA>
## 256                 JJ Powell   Male                    <NA>              <NA>
## 257             Johann Krauss   Male                    <NA>              <NA>
## 258          John Constantine   Male                    blue             Human
## 259              John Stewart   Male                   green             Human
## 260               John Wraith   Male                   brown              <NA>
## 261                      Jolt Female                    blue              <NA>
## 262                   Jubilee Female                     red            Mutant
## 263               Judge Dredd   Male                    <NA>             Human
## 264                   Justice   Male                   hazel             Human
## 265                  Jyn Erso Female                   green             Human
## 266                     K-2SO   Male                   white           Android
## 267                Karate Kid   Male                   brown             Human
## 268           Kathryn Janeway Female                    <NA>             Human
## 269          Katniss Everdeen Female                    <NA>             Human
## 270                  Kevin 11   Male                    <NA>             Human
## 271                  Kick-Ass   Male                    blue             Human
## 272                 Kid Flash   Male                   green             Human
## 273              Kid Flash II   Male                    <NA>              <NA>
## 274                   Kilowog   Male                     red        Bolovaxian
## 275                 King Kong   Male                  yellow            Animal
## 276              Kool-Aid Man   Male                   black              <NA>
## 277                    Krypto   Male                    blue        Kryptonian
## 278               Kyle Rayner   Male                   green             Human
## 279                     Leech   Male                    <NA>              <NA>
## 280                    Legion   Male            green / blue            Mutant
## 281                  Leonardo   Male                    blue            Mutant
## 282                Light Lass Female                    blue              <NA>
## 283             Lightning Lad   Male                    blue              <NA>
## 284               Liz Sherman Female                    <NA>              <NA>
## 285                  Longshot   Male                    blue             Human
## 286                 Luke Cage   Male                   brown             Human
## 287            Luke Skywalker   Male                    blue             Human
## 288                      Luna Female                    <NA>             Human
## 289                      Lyja Female                   green              <NA>
## 290               Machine Man   <NA>                     red              <NA>
## 291                     Magog   Male                    blue              <NA>
## 292                 Man-Thing   Male                     red              <NA>
## 293                  Man-Wolf   Male                   brown              <NA>
## 294                    Mantis Female                   green        Human-Kree
## 295         Martian Manhunter   Male                     red           Martian
## 296               Marvel Girl Female                   green              <NA>
## 297              Master Brood   Male                    blue              <NA>
## 298              Master Chief   Male                   brown   Human / Altered
## 299              Matt Parkman   Male                    <NA>              <NA>
## 300                  Maverick   Male                    blue              <NA>
## 301              Maya Herrera Female                    <NA>              <NA>
## 302                    Medusa Female                   green           Inhuman
## 303                  Meltdown Female                    blue              <NA>
## 304                      Mera Female                    blue         Atlantean
## 305                Metamorpho   Male                   black              <NA>
## 306                 Meteorite Female                    <NA>              <NA>
## 307                    Metron   Male                    blue              <NA>
## 308             Micah Sanders   Male                   brown              <NA>
## 309              Michelangelo   Male                    blue            Mutant
## 310                 Micro Lad   Male                    grey              <NA>
## 311                     Mimic   Male                   brown              <NA>
## 312              Minna Murray Female                    <NA>              <NA>
## 313                    Misfit Female                    blue              <NA>
## 314              Miss Martian Female                     red              <NA>
## 315          Mister Fantastic   Male                   brown Human / Radiation
## 316               Mockingbird Female                    blue             Human
## 317                      Mogo   Male                    <NA>            Planet
## 318           Mohinder Suresh   Male                    <NA>              <NA>
## 319                   Monarch   Male                    blue              <NA>
## 320             Monica Dawson Female                    <NA>              <NA>
## 321               Moon Knight   Male                   brown             Human
## 322                     Morph   Male                   white              <NA>
## 323               Mr Immortal   Male                    blue            Mutant
## 324             Mr Incredible   Male                    blue             Human
## 325              Ms Marvel II Female                    blue              <NA>
## 326              Multiple Man   Male                    blue              <NA>
## 327                     Namor   Male                    <NA>              <NA>
## 328                     Namor   Male                    grey         Atlantean
## 329                    Namora Female                    blue              <NA>
## 330                  Namorita Female                    blue              <NA>
## 331            Naruto Uzumaki   Male                    <NA>             Human
## 332           Nathan Petrelli   Male                   brown              <NA>
## 333 Negasonic Teenage Warhead Female                   black            Mutant
## 334                 Nick Fury   Male                   brown             Human
## 335              Nightcrawler   Male                  yellow              <NA>
## 336                 Nightwing   Male                    blue             Human
## 337              Niki Sanders Female                    blue              <NA>
## 338              Nina Theroux Female                    <NA>             Alpha
## 339               Nite Owl II   Male                    <NA>              <NA>
## 340                 Northstar   Male                    blue              <NA>
## 341                      Nova   Male                   brown             Human
## 342                      Nova Female                   white    Human / Cosmic
## 343                      Odin   Male                    blue     God / Eternal
## 344                 Offspring   Male                    <NA>              <NA>
## 345                Omniscient   Male                   brown              <NA>
## 346             One Punch Man   Male                    <NA>             Human
## 347                    Oracle Female                    blue             Human
## 348                    Osiris   Male                   brown              <NA>
## 349                Paul Blart   Male                    <NA>             Human
## 350                   Penance   <NA>                    <NA>              <NA>
## 351                 Penance I Female                    <NA>              <NA>
## 352                Penance II   Male                    blue              <NA>
## 353            Peter Petrelli   Male                    <NA>              <NA>
## 354                   Phantom   Male                    <NA>              <NA>
## 355              Phantom Girl Female                    blue              <NA>
## 356                   Phoenix Female                   green            Mutant
## 357               Plastic Lad   Male                    <NA>              <NA>
## 358               Plastic Man   Male                    blue             Human
## 359                   Polaris Female                   green            Mutant
## 360                Power Girl Female                    blue        Kryptonian
## 361                 Power Man   Male                    <NA>            Mutant
## 362               Professor X   Male                    blue            Mutant
## 363                  Psylocke Female                    blue            Mutant
## 364                  Punisher   Male                    blue             Human
## 365                   Quantum   Male                    <NA>              <NA>
## 366                  Question   Male                    blue             Human
## 367               Quicksilver   Male                    blue            Mutant
## 368                     Quill   Male                   brown              <NA>
## 369             Rachel Pirzad Female                    <NA>             Alpha
## 370                     Rambo   Male                   brown             Human
## 371                   Raphael   Male                    <NA>            Mutant
## 372                       Ray   Male                   green             Human
## 373                 Red Arrow   Male                   green             Human
## 374                 Red Robin   Male                    blue             Human
## 375               Red Tornado   Male                   green           Android
## 376              Renata Soliz Female                    <NA>              <NA>
## 377                       Rey Female                   hazel             Human
## 378                Rip Hunter   Male                    blue             Human
## 379                   Ripcord Female                   green              <NA>
## 380                     Robin   Male                    blue             Human
## 381                  Robin II   Male                    blue             Human
## 382                 Robin III   Male                    blue             Human
## 383                   Robin V   Male                    blue             Human
## 384            Rocket Raccoon   Male                   brown            Animal
## 385                     Rogue Female                   green              <NA>
## 386                     Ronin   Male                    blue             Human
## 387                 Rorschach   Male                    blue             Human
## 388                      Sage Female                    blue              <NA>
## 389                 Sasquatch   Male                     red              <NA>
## 390             Savage Dragon   Male                    <NA>              <NA>
## 391            Scarlet Spider   Male                    blue             Human
## 392         Scarlet Spider II   Male                   brown             Clone
## 393               Shadow King   <NA>                     red              <NA>
## 394               Shadow Lass Female                   black          Talokite
## 395                 Shadowcat Female                   hazel            Mutant
## 396                 Shang-Chi   Male                   brown             Human
## 397               Shatterstar   Male                   brown              <NA>
## 398                  She-Hulk Female                   green             Human
## 399                 She-Thing Female                    blue Human / Radiation
## 400                    Shriek Female           yellow / blue              <NA>
## 401          Shrinking Violet Female                    <NA>              <NA>
## 402                       Sif Female                    blue         Asgardian
## 403                      Silk Female                   brown             Human
## 404              Silk Spectre Female                    <NA>              <NA>
## 405           Silk Spectre II Female                    <NA>              <NA>
## 406             Silver Surfer   Male                   white             Alien
## 407                Silverclaw Female                   brown              <NA>
## 408                 Simon Baz   Male                    bown             Human
## 409                     Skaar   Male                   green              <NA>
## 410                  Snowbird Female                   white              <NA>
## 411                     Sobek   Male                   white              <NA>
## 412                  Songbird Female                   green              <NA>
## 413               Space Ghost   Male                    <NA>             Human
## 414                     Spawn   Male                   brown             Demon
## 415                   Spectre   Male                   white     God / Eternal
## 416                 Speedball   Male                    <NA>              <NA>
## 417                    Speedy   Male                    <NA>             Human
## 418                    Speedy Female                   green             Human
## 419               Spider-Girl Female                    blue             Human
## 420               Spider-Gwen Female                    blue             Human
## 421                Spider-Man   Male                   hazel             Human
## 422                Spider-Man   <NA>                     red             Human
## 423                Spider-Man   Male                   brown             Human
## 424              Spider-Woman Female                   green             Human
## 425           Spider-Woman II Female                    <NA>              <NA>
## 426          Spider-Woman III Female                   brown              <NA>
## 427                     Spock   Male                   brown      Human-Vulcan
## 428                     Spyke   Male                   brown            Mutant
## 429                   Stacy X Female                    <NA>              <NA>
## 430                 Star-Lord   Male                    blue     Human-Spartoi
## 431                  Stardust   Male                    <NA>              <NA>
## 432                  Starfire Female                   green        Tamaranean
## 433                  Stargirl Female                    blue             Human
## 434                    Static   Male                   brown            Mutant
## 435                     Steel   Male                   brown              <NA>
## 436          Stephanie Powell Female                    <NA>              <NA>
## 437                     Storm Female                    blue            Mutant
## 438                   Sunspot   Male                   brown            Mutant
## 439                  Superboy   Male                    blue              <NA>
## 440                 Supergirl Female                    blue        Kryptonian
## 441                  Superman   Male                    blue        Kryptonian
## 442                     Synch   Male                   brown              <NA>
## 443                   Tempest Female                   brown              <NA>
## 444                  The Cape   Male                    <NA>              <NA>
## 445                     Thing   Male                    blue Human / Radiation
## 446                      Thor   Male                    blue         Asgardian
## 447                 Thor Girl Female                    blue         Asgardian
## 448               Thunderbird   Male                   brown              <NA>
## 449            Thunderbird II   Male                    <NA>              <NA>
## 450           Thunderbird III   Male                   brown              <NA>
## 451             Thunderstrike   Male                    blue              <NA>
## 452                   Thundra Female                   green              <NA>
## 453                     Tigra Female                   green              <NA>
## 454                     Titan   Male                    <NA>              <NA>
## 455                     Toxin   Male                    blue          Symbiote
## 456                     Toxin   Male                   black          Symbiote
## 457             Tracy Strauss Female                    <NA>              <NA>
## 458           Triplicate Girl Female                  purple              <NA>
## 459                    Triton   Male                   green           Inhuman
## 460                 Ultragirl Female                    blue              <NA>
## 461                  Vagabond Female                    blue              <NA>
## 462              Valerie Hart Female                   hazel              <NA>
## 463                  Valkyrie Female                    blue              <NA>
## 464                Vertigo II Female                    blue              <NA>
## 465                      Vibe   Male                   brown             Human
## 466                Vindicator Female                   green             Human
## 467                Vindicator   Male                    <NA>              <NA>
## 468               Violet Parr Female                  violet             Human
## 469                    Vision   Male                    gold           Android
## 470                 Vision II   <NA>                     red              <NA>
## 471                     Vixen Female                   amber             Human
## 472                    Vulcan   Male                   black              <NA>
## 473               War Machine   Male                   brown             Human
## 474                   Warbird Female                    blue              <NA>
## 475                   Warlock   Male                     red              <NA>
## 476                   Warpath   Male                   brown            Mutant
## 477                      Wasp Female                    blue             Human
## 478                   Watcher   Male                    <NA>              <NA>
## 479               White Queen Female                    blue              <NA>
## 480                  Wildfire   Male                    <NA>              <NA>
## 481            Winter Soldier   Male                   brown             Human
## 482                   Wiz Kid   <NA>                   brown              <NA>
## 483                 Wolfsbane Female                   green              <NA>
## 484                 Wolverine   Male                    blue            Mutant
## 485               Wonder Girl Female                    blue          Demi-God
## 486                Wonder Man   Male                     red              <NA>
## 487              Wonder Woman Female                    blue            Amazon
## 488                    Wondra Female                    <NA>              <NA>
## 489            Wyatt Wingfoot   Male                   brown              <NA>
## 490                      X-23 Female                   green    Mutant / Clone
## 491                     X-Man   Male                    blue              <NA>
## 492              Yellowjacket   Male                    blue             Human
## 493           Yellowjacket II Female                    blue             Human
## 494                      Ymir   Male                   white       Frost Giant
## 495                      Yoda   Male                   brown    Yoda's species
## 496                   Zatanna Female                    blue             Human
##           hair_color height         publisher     skin_color alignment weight
## 1            No Hair  203.0     Marvel Comics           <NA>      good    441
## 2            No Hair  191.0 Dark Horse Comics           blue      good     65
## 3            No Hair  185.0         DC Comics            red      good     90
## 4              Blond     NA      NBC - Heroes           <NA>      good     NA
## 5              Blond  185.0         DC Comics           <NA>      good     88
## 6              Blond  173.0     Marvel Comics           <NA>      good     61
## 7              Brown  178.0     Marvel Comics           <NA>      good     81
## 8               <NA>  191.0     Marvel Comics           <NA>      good    104
## 9              Blond  180.0         DC Comics           <NA>      good     90
## 10              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 11             Black  178.0         DC Comics           <NA>      good     72
## 12              <NA>     NA         Wildstorm           <NA>      good     NA
## 13              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 14             Blond  183.0     Marvel Comics           <NA>      good     68
## 15              <NA>     NA Dark Horse Comics           <NA>      good     NA
## 16             Black  165.0     Marvel Comics           <NA>      good     57
## 17             Black  163.0     Marvel Comics           <NA>      good     54
## 18             Blond  183.0         DC Comics           <NA>      good     83
## 19             Blond  211.0     Marvel Comics           <NA>      good    122
## 20             Blond  183.0     Marvel Comics           <NA>      good     86
## 21             Blond     NA         DC Comics           <NA>      good     NA
## 22             Black  178.0         DC Comics           <NA>      good    106
## 23             Blond  185.0         DC Comics           <NA>      good    146
## 24             Blond  175.0     Marvel Comics           <NA>      good     63
## 25             Blond  183.0     Marvel Comics           blue      good     68
## 26            Orange  193.0     Marvel Comics           gold      good     98
## 27             Brown  185.0     Marvel Comics           <NA>      good    270
## 28              Pink  165.0     Marvel Comics           <NA>      good     59
## 29             Black  163.0     Marvel Comics           <NA>      good     50
## 30              <NA>     NA         DC Comics           <NA>      good     NA
## 31             Black     NA              <NA>           <NA>      good     NA
## 32               Red  183.0     Marvel Comics           <NA>      good    101
## 33               Red  178.0         DC Comics           <NA>      good     68
## 34              <NA>     NA         DC Comics           <NA>      good     NA
## 35             Black  168.0         DC Comics           <NA>      good     54
## 36            Auburn  183.0         DC Comics           <NA>      good     81
## 37               Red     NA         DC Comics           <NA>      good     NA
## 38             Black     NA         DC Comics           <NA>      good     72
## 39             Black  180.0     Marvel Comics           <NA>      good     63
## 40             Black     NA         DC Comics           <NA>      good     NA
## 41              <NA>     NA         DC Comics           <NA>      good     NA
## 42  Strawberry Blond  183.0     Marvel Comics           <NA>      good     77
## 43             Black  165.0     Marvel Comics           <NA>      good     54
## 44              <NA>     NA         DC Comics           <NA>      good     NA
## 45               Red  170.0         DC Comics           <NA>      good     57
## 46              <NA>     NA         DC Comics           <NA>      good     NA
## 47             Black  165.0         DC Comics           <NA>      good     52
## 48              <NA>     NA         DC Comics           <NA>      good     NA
## 49             Blond  168.0         DC Comics           <NA>      good     61
## 50             black  188.0         DC Comics           <NA>      good     95
## 51             Black  178.0         DC Comics           <NA>      good     77
## 52             Black  178.0         DC Comics           <NA>      good     79
## 53             Black  198.0     Marvel Comics           <NA>      good    133
## 54               Red  178.0         DC Comics           <NA>      good     NA
## 55             White  175.0     Marvel Comics           <NA>      good     63
## 56              Blue  180.0     Marvel Comics           blue      good    181
## 57             Green  173.0         DC Comics          green      good     68
## 58              <NA>     NA         DC Comics           <NA>      good     NA
## 59           No Hair  201.0     Marvel Comics           <NA>      good    216
## 60              <NA>     NA     Marvel Comics           <NA>      good     NA
## 61              <NA>     NA       Icon Comics           <NA>      good     NA
## 62              <NA>     NA              SyFy           <NA>      good     NA
## 63             Blond  180.0     Marvel Comics           <NA>      good     54
## 64             Black     NA              <NA>           <NA>      good     NA
## 65              <NA>     NA     Marvel Comics           <NA>      good     NA
## 66              <NA>     NA     Hanna-Barbera           <NA>      good     NA
## 67           No Hair  198.0     Marvel Comics           <NA>      good    124
## 68             Black  188.0     Marvel Comics           <NA>      good     95
## 69             Blond  165.0         DC Comics           <NA>      good     58
## 70             Blond  170.0         DC Comics           <NA>      good     59
## 71             Blond  178.0     Marvel Comics           <NA>      good     54
## 72              <NA>     NA     Marvel Comics           <NA>      good     NA
## 73             Brown  183.0     Marvel Comics           <NA>      good     86
## 74           No Hair  185.0         DC Comics           <NA>      good     90
## 75             Black  183.0     Marvel Comics           <NA>      good     90
## 76            Auburn  170.0     Marvel Comics           <NA>      good     59
## 77             Blond  170.0     Marvel Comics           <NA>      good     61
## 78             Black  188.0     Marvel Comics           <NA>      good     97
## 79           No Hair     NA     Marvel Comics           <NA>      good     NA
## 80              <NA>  168.0     Marvel Comics           <NA>      good     68
## 81           Magenta  165.0     Marvel Comics           pink      good     56
## 82           No Hair     NA     Marvel Comics           <NA>      good     NA
## 83             Brown     NA         DC Comics           <NA>      good     NA
## 84              <NA>     NA         DC Comics           <NA>      good     NA
## 85             Brown  183.0         DC Comics           <NA>      good     86
## 86             Black     NA         DC Comics           <NA>      good     NA
## 87              <NA>     NA     Marvel Comics           <NA>      good     NA
## 88             Blond  165.0     Marvel Comics           <NA>      good     55
## 89              <NA>     NA     Marvel Comics           <NA>      good     NA
## 90             Blond  196.0         DC Comics           <NA>      good     97
## 91              <NA>     NA     Marvel Comics           <NA>      good     NA
## 92             Blond  193.0     Marvel Comics           <NA>      good    110
## 93     Brown / Black     NA     Marvel Comics           <NA>      good     NA
## 94             Blond  170.0         DC Comics           <NA>      good     61
## 95     Brown / White  183.0     Marvel Comics           <NA>      good     99
## 96             Blond  157.0 Dark Horse Comics           <NA>      good     52
## 97             Black  170.0         DC Comics           <NA>      good     59
## 98              <NA>     NA     Marvel Comics           <NA>      good     NA
## 99              <NA>     NA         DC Comics           <NA>      good     NA
## 100            White  203.0     Marvel Comics           <NA>      good    158
## 101             <NA>     NA              SyFy           <NA>      good     NA
## 102            Blond  183.0     Marvel Comics           <NA>      good     81
## 103            blond  188.0     Marvel Comics           <NA>      good    108
## 104           Silver  193.0         DC Comics         silver      good     90
## 105            Blond  198.0     Marvel Comics           <NA>      good    116
## 106            Brown  188.0      Team Epic TV           <NA>      good     NA
## 107            Black     NA        South Park           <NA>      good     NA
## 108            Blond  188.0     Marvel Comics           <NA>      good    108
## 109            Blond  180.0     Marvel Comics           <NA>      good     74
## 110            Black  193.0         DC Comics           <NA>      good    101
## 111            Black  175.0         DC Comics           <NA>      good     74
## 112             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 113            Green     NA     Marvel Comics           <NA>      good     NA
## 114             <NA>     NA     Marvel Comics           <NA>      good     NA
## 115            Blond  173.0     Marvel Comics           <NA>      good     61
## 116             <NA>     NA     Marvel Comics           <NA>      good     NA
## 117            Black  175.0         DC Comics           <NA>      good     61
## 118            Brown  170.0     Marvel Comics           <NA>      good     62
## 119            White  201.0     Marvel Comics           grey      good     97
## 120             <NA>     NA     Marvel Comics           <NA>      good     NA
## 121            Brown  175.0     Marvel Comics           <NA>      good     63
## 122             <NA>  178.0              <NA>           <NA>      good     NA
## 123              Red  183.0         DC Comics           <NA>      good    170
## 124            Blond     NA      NBC - Heroes           <NA>      good     NA
## 125            White     NA     Marvel Comics           <NA>      good     NA
## 126            black  226.0     Marvel Comics           <NA>      good     70
## 127            Brown     NA     HarperCollins           <NA>      good     NA
## 128             <NA>     NA         DC Comics           <NA>      good     NA
## 129            Black  226.0     Marvel Comics           <NA>      good    225
## 130            Brown  191.0     Marvel Comics           <NA>      good     79
## 131 Strawberry Blond     NA     Marvel Comics           <NA>      good     NA
## 132          No Hair  180.0     Marvel Comics           <NA>      good    104
## 133              Red  168.0     Marvel Comics           <NA>      good     50
## 134            Black  198.0         DC Comics           <NA>      good    173
## 135            Brown  191.0     Marvel Comics           <NA>      good     88
## 136            Blond  175.0     Marvel Comics           <NA>      good     68
## 137            Blond  165.0     Marvel Comics           <NA>      good     52
## 138            Blond     NA     HarperCollins           <NA>      good     NA
## 139             <NA>     NA       ABC Studios           <NA>      good     NA
## 140              Red  183.0     Marvel Comics           <NA>      good     90
## 141            Brown  185.0     Marvel Comics           <NA>      good     81
## 142             <NA>     NA Universal Studios           <NA>      good     NA
## 143            Blond  168.0     Marvel Comics           <NA>      good     56
## 144            Blond  122.0 Dark Horse Comics           <NA>      good     27
## 145            Brown     NA         Star Trek           <NA>      good     NA
## 146            Blond  173.0     Marvel Comics           <NA>      good     52
## 147            Black  183.0         DC Comics           <NA>      good     90
## 148             Grey  193.0     Marvel Comics           <NA>      good    178
## 149             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 150            Green  198.0     Marvel Comics           <NA>      good    171
## 151            Blond  188.0         DC Comics           <NA>      good     89
## 152            Black  188.0     Marvel Comics           <NA>      good     81
## 153            Black  173.0     Marvel Comics          white      good     54
## 154          No Hair     NA    IDW Publishing          green      good     NA
## 155            Black  175.0         DC Comics           <NA>      good     63
## 156          No Hair     NA         DC Comics           blue      good     NA
## 157          No Hair  193.0     Marvel Comics          green      good    306
## 158            Brown  168.0 Dark Horse Comics           <NA>      good     56
## 159            Black  175.0     Marvel Comics           <NA>      good     59
## 160              Red  185.0         DC Comics           <NA>      good     80
## 161            Blond  178.0     Marvel Comics           <NA>      good     65
## 162            Blond  168.0         DC Comics           <NA>      good     57
## 163             <NA>     NA     HarperCollins           <NA>      good     NA
## 164             <NA>     NA         DC Comics           <NA>      good     NA
## 165            Brown  168.0              <NA>           <NA>      good     NA
## 166            Black  188.0     Marvel Comics           <NA>      good    108
## 167   Orange / White  175.0     Marvel Comics           <NA>      good     50
## 168              Red     NA         DC Comics           <NA>      good     NA
## 169          No Hair  975.0     Marvel Comics          green      good     18
## 170            Black  165.0     Marvel Comics           <NA>      good     56
## 171           Yellow  193.0     Marvel Comics           <NA>      good     99
## 172              Red  173.0     Marvel Comics           <NA>      good     56
## 173            Black     NA         DC Comics           <NA>      good     NA
## 174           Auburn  188.0         DC Comics           <NA>      good     91
## 175    Brown / White  180.0         DC Comics           <NA>      good     81
## 176             <NA>     NA              <NA>           <NA>      good     NA
## 177            Blond  183.0         DC Comics           <NA>      good     88
## 178             <NA>  183.0         DC Comics           <NA>      good     86
## 179           Auburn  157.0         DC Comics           <NA>      good     52
## 180            Black  183.0     Marvel Comics           <NA>      good     81
## 181            Blond  142.0     Marvel Comics           <NA>      good     45
## 182             Grey  188.0     Marvel Comics           <NA>      good     92
## 183            White  180.0     Marvel Comics           <NA>      good    167
## 184            Brown  185.0     Marvel Comics           <NA>      good     81
## 185            Black  183.0     Marvel Comics          green      good     77
## 186             <NA>     NA         DC Comics           <NA>      good     NA
## 187             <NA>     NA              SyFy           <NA>      good     NA
## 188            Blond  185.0     Marvel Comics           <NA>      good     86
## 189          No Hair  188.0     Marvel Comics           <NA>      good     99
## 190             <NA>     NA     Marvel Comics           <NA>      good     NA
## 191             <NA>     NA     Marvel Comics           <NA>      good     NA
## 192             <NA>     NA     Marvel Comics           <NA>      good     NA
## 193             <NA>  175.0          Shueisha           <NA>      good     62
## 194             <NA>     NA     Marvel Comics           <NA>      good     NA
## 195             <NA>     NA     Marvel Comics           <NA>      good     NA
## 196             <NA>     NA     Marvel Comics           <NA>      good     NA
## 197            Black  183.0     Marvel Comics           <NA>      good     90
## 198            Brown  178.0     Marvel Comics           <NA>      good     79
## 199            Blond  188.0         DC Comics           <NA>      good     88
## 200             <NA>  183.0     Marvel Comics           <NA>      good     88
## 201            Brown  178.0     Marvel Comics           <NA>      good     79
## 202             <NA>  701.0     Marvel Comics           <NA>      good      4
## 203            Black     NA     Marvel Comics           <NA>      good     NA
## 204              Red  188.0         DC Comics           <NA>      good     95
## 205            Brown  188.0         DC Comics           <NA>      good     90
## 206            Brown  183.0      George Lucas           <NA>      good     79
## 207            Black  188.0     Sony Pictures           <NA>      good     NA
## 208            Black     NA     J. K. Rowling           <NA>      good     NA
## 209            Blond  183.0     Marvel Comics           <NA>      good     79
## 210            Brown  185.0         DC Comics           <NA>      good     89
## 211            Blond  191.0     Marvel Comics           <NA>      good    104
## 212            Black  165.0     Marvel Comics           <NA>      good     57
## 213              Red  175.0         DC Comics           <NA>      good     61
## 214            Brown  185.0         DC Comics           <NA>      good     88
## 215              Red  175.0         DC Comics           <NA>      good     54
## 216             <NA>     NA         DC Comics           <NA>      good     NA
## 217              Red  170.0         DC Comics           <NA>      good     65
## 218            Black  259.0 Dark Horse Comics           <NA>      good    158
## 219              Red  173.0     Marvel Comics           <NA>      good     61
## 220              Red  185.0     Marvel Comics           <NA>      good     81
## 221            Brown  196.0     Marvel Comics           <NA>      good    146
## 222             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 223             <NA>     NA       Icon Comics           <NA>      good     NA
## 224              Red  170.0     Marvel Comics           <NA>      good     NA
## 225              Red  168.0     Marvel Comics           <NA>      good     48
## 226           Yellow   79.0     Marvel Comics           <NA>      good     18
## 227            Green  244.0     Marvel Comics          green      good    630
## 228            Blond  178.0     Marvel Comics           <NA>      good     77
## 229            Black  180.0         DC Comics           <NA>      good     59
## 230            Blond  170.0     Marvel Comics           <NA>      good     58
## 231            Black  175.0     Marvel Comics           <NA>      good     77
## 232              Red  183.0     Marvel Comics           <NA>      good    207
## 233            Brown  173.0     Marvel Comics           <NA>      good     65
## 234           Auburn  170.0         DC Comics           <NA>      good     65
## 235             <NA>  183.0      George Lucas           <NA>      good     79
## 236          No Hair  180.0     Marvel Comics           <NA>      good     81
## 237            Blond  168.0     Marvel Comics           <NA>      good     54
## 238            Blond  180.0     Marvel Comics           <NA>      good     79
## 239            Black  198.0     Marvel Comics           <NA>      good    191
## 240             <NA>     NA         DC Comics           <NA>      good     NA
## 241             <NA>     NA              <NA>           <NA>      good     NA
## 242            Brown  155.0     Marvel Comics           <NA>      good     79
## 243            Brown   71.0 Dark Horse Comics           <NA>      good     14
## 244            Blond  183.0       Titan Books           <NA>      good     NA
## 245            Brown  178.0         Star Trek           <NA>      good     77
## 246             <NA>  193.0      George Lucas orange / white      good     NA
## 247             <NA>     NA              <NA>           <NA>      good     NA
## 248              Red  168.0     Marvel Comics           <NA>      good     52
## 249             <NA>     NA         Star Trek           <NA>      good     NA
## 250            Blond  168.0     Marvel Comics           <NA>      good     55
## 251             <NA>     NA         DC Comics           <NA>      good     NA
## 252            Brown     NA         DC Comics           <NA>      good     NA
## 253            Brown  170.0     Marvel Comics           <NA>      good     56
## 254             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 255             <NA>     NA       ABC Studios           <NA>      good     NA
## 256             <NA>     NA       ABC Studios           <NA>      good     NA
## 257             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 258            Blond  183.0         DC Comics           <NA>      good     NA
## 259            Black  185.0         DC Comics           <NA>      good     90
## 260            Black  183.0     Marvel Comics           <NA>      good     88
## 261            Black  165.0     Marvel Comics           <NA>      good     49
## 262            Black  165.0     Marvel Comics           <NA>      good     52
## 263             <NA>  188.0         Rebellion           <NA>      good     NA
## 264            Brown  178.0     Marvel Comics           <NA>      good     81
## 265            Brown     NA      George Lucas           <NA>      good     NA
## 266          No Hair  213.0      George Lucas           gray      good     NA
## 267            Brown  173.0         DC Comics           <NA>      good     72
## 268             <NA>     NA         Star Trek           <NA>      good     NA
## 269             <NA>     NA              <NA>           <NA>      good     NA
## 270            Black     NA         DC Comics           <NA>      good     NA
## 271            Blond     NA       Icon Comics           <NA>      good     NA
## 272              Red     NA         DC Comics           <NA>      good     NA
## 273             <NA>     NA         DC Comics           <NA>      good     NA
## 274          No Hair  234.0         DC Comics           pink      good    324
## 275            Black   30.5              <NA>           <NA>      good     NA
## 276          No Hair     NA              <NA>            red      good     NA
## 277            White   64.0         DC Comics           <NA>      good     18
## 278            Black  180.0         DC Comics           <NA>      good     79
## 279             <NA>     NA     Marvel Comics           <NA>      good     NA
## 280            Black  175.0     Marvel Comics           <NA>      good     59
## 281          No Hair     NA    IDW Publishing          green      good     NA
## 282              Red  165.0         DC Comics           <NA>      good     54
## 283              Red  155.0         DC Comics           <NA>      good     65
## 284             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 285            Blond  188.0     Marvel Comics           <NA>      good     36
## 286            Black  198.0     Marvel Comics           <NA>      good    191
## 287            Blond  168.0      George Lucas           <NA>      good     77
## 288             <NA>     NA     Marvel Comics           <NA>      good     NA
## 289            Green     NA     Marvel Comics           <NA>      good     NA
## 290            Black  183.0     Marvel Comics           <NA>      good    383
## 291            Blond     NA         DC Comics           <NA>      good     NA
## 292          No Hair  213.0     Marvel Comics          green      good    225
## 293           Auburn  188.0     Marvel Comics           <NA>      good     90
## 294            Black  168.0     Marvel Comics          green      good     52
## 295          No Hair  201.0         DC Comics          green      good    135
## 296              Red  170.0     Marvel Comics           <NA>      good     56
## 297            Black  183.0      Team Epic TV           <NA>      good     81
## 298            Brown  213.0         Microsoft           <NA>      good     NA
## 299             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 300            Black  193.0     Marvel Comics           <NA>      good    110
## 301             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 302              Red  180.0     Marvel Comics           <NA>      good     59
## 303            Blond  165.0     Marvel Comics           <NA>      good     54
## 304              Red  175.0         DC Comics           <NA>      good     72
## 305          No Hair  185.0         DC Comics           <NA>      good     90
## 306             <NA>     NA     Marvel Comics           <NA>      good     NA
## 307            Black  185.0         DC Comics           <NA>      good     86
## 308            Black     NA      NBC - Heroes           <NA>      good     NA
## 309             <NA>     NA    IDW Publishing          green      good     NA
## 310            Brown  183.0         DC Comics           <NA>      good     77
## 311            Brown  188.0     Marvel Comics           <NA>      good    101
## 312             <NA>     NA         Wildstorm           <NA>      good     NA
## 313              Red     NA         DC Comics           <NA>      good     NA
## 314              Red  178.0         DC Comics           <NA>      good     61
## 315            Brown  185.0     Marvel Comics           <NA>      good     81
## 316            Blond  175.0     Marvel Comics           <NA>      good     61
## 317             <NA>     NA         DC Comics           <NA>      good     NA
## 318             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 319            White  193.0         DC Comics           <NA>      good     90
## 320             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 321            Brown  188.0     Marvel Comics           <NA>      good    101
## 322          No Hair  178.0     Marvel Comics           <NA>      good     79
## 323            Blond  188.0     Marvel Comics           <NA>      good     70
## 324            Blond  201.0 Dark Horse Comics           <NA>      good    158
## 325              Red  173.0     Marvel Comics           <NA>      good     61
## 326            Brown  180.0     Marvel Comics           <NA>      good     70
## 327             <NA>     NA     Marvel Comics           <NA>      good     NA
## 328            Black  188.0     Marvel Comics           <NA>      good    125
## 329            Blond  180.0     Marvel Comics           <NA>      good     85
## 330            Blond  168.0     Marvel Comics           <NA>      good    101
## 331             <NA>  168.0          Shueisha           <NA>      good     54
## 332             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 333            Black     NA     Marvel Comics           <NA>      good     NA
## 334    Brown / White  185.0     Marvel Comics           <NA>      good     99
## 335           Indigo  175.0     Marvel Comics           <NA>      good     88
## 336            Black  178.0         DC Comics           <NA>      good     79
## 337            Blond     NA      NBC - Heroes           <NA>      good     NA
## 338             <NA>     NA              SyFy           <NA>      good     NA
## 339             <NA>     NA         DC Comics           <NA>      good     NA
## 340            Black  180.0     Marvel Comics           <NA>      good     83
## 341            Brown  185.0     Marvel Comics           <NA>      good     86
## 342              Red  163.0     Marvel Comics           gold      good     59
## 343            White  206.0     Marvel Comics           <NA>      good    293
## 344             <NA>     NA         DC Comics           <NA>      good     NA
## 345            Black  180.0      Team Epic TV           <NA>      good     65
## 346          No Hair  175.0          Shueisha           <NA>      good     69
## 347              Red  178.0         DC Comics           <NA>      good     59
## 348            Brown     NA         DC Comics           <NA>      good     NA
## 349             <NA>  170.0     Sony Pictures           <NA>      good    117
## 350             <NA>     NA     Marvel Comics           <NA>      good     NA
## 351             <NA>     NA     Marvel Comics           <NA>      good     NA
## 352            Blond  183.0     Marvel Comics           <NA>      good     89
## 353             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 354             <NA>     NA         DC Comics           <NA>      good     NA
## 355            Black  168.0         DC Comics           <NA>      good     54
## 356              Red  168.0     Marvel Comics           <NA>      good     52
## 357             <NA>     NA         DC Comics           <NA>      good     NA
## 358            Black  185.0         DC Comics           <NA>      good     80
## 359            Green  170.0     Marvel Comics           <NA>      good     52
## 360            blond  180.0         DC Comics           <NA>      good     81
## 361             <NA>     NA     Marvel Comics           <NA>      good     NA
## 362          No Hair  183.0     Marvel Comics           <NA>      good     86
## 363           Purple  180.0     Marvel Comics           <NA>      good     70
## 364            Black  183.0     Marvel Comics           <NA>      good     90
## 365             <NA>     NA     HarperCollins           <NA>      good     NA
## 366            Blond  188.0         DC Comics           <NA>      good     83
## 367           Silver  183.0     Marvel Comics           <NA>      good     79
## 368            Brown  163.0     Marvel Comics           <NA>      good     56
## 369             <NA>     NA              SyFy           <NA>      good     NA
## 370            Black  178.0              <NA>           <NA>      good     83
## 371          No Hair     NA    IDW Publishing          green      good     NA
## 372              Red  178.0         DC Comics           <NA>      good     70
## 373              Red  180.0         DC Comics           <NA>      good     83
## 374            Black  165.0         DC Comics           <NA>      good     56
## 375          No Hair  185.0         DC Comics           <NA>      good    146
## 376             <NA>     NA     HarperCollins           <NA>      good     NA
## 377            Brown  297.0      George Lucas           <NA>      good     NA
## 378            Blond     NA         DC Comics           <NA>      good     NA
## 379            Black  180.0     Marvel Comics           <NA>      good     72
## 380            Black  178.0         DC Comics           <NA>      good     79
## 381              Red  183.0         DC Comics           <NA>      good    101
## 382            Black  165.0         DC Comics           <NA>      good     56
## 383            Black  137.0         DC Comics           <NA>      good     38
## 384            Brown  122.0     Marvel Comics           <NA>      good     25
## 385    Brown / White  173.0     Marvel Comics           <NA>      good     54
## 386            Blond  191.0     Marvel Comics           <NA>      good    104
## 387              Red  168.0         DC Comics           <NA>      good     63
## 388            Black  170.0     Marvel Comics           <NA>      good     61
## 389           Orange  305.0     Marvel Comics           <NA>      good    900
## 390             <NA>     NA      Image Comics           <NA>      good     NA
## 391            Blond  178.0     Marvel Comics           <NA>      good     74
## 392            Brown  193.0     Marvel Comics           <NA>      good    113
## 393             <NA>  185.0     Marvel Comics           <NA>      good    149
## 394            Black  173.0         DC Comics           blue      good     54
## 395            Brown  168.0     Marvel Comics           <NA>      good     50
## 396            Black  178.0     Marvel Comics           <NA>      good     79
## 397              Red  191.0     Marvel Comics           <NA>      good     88
## 398            Green  201.0     Marvel Comics           <NA>      good    315
## 399          No Hair  183.0     Marvel Comics           <NA>      good    153
## 400            Black  173.0     Marvel Comics           <NA>      good     52
## 401             <NA>     NA         DC Comics           <NA>      good     NA
## 402            Black  188.0     Marvel Comics           <NA>      good    191
## 403            Black     NA     Marvel Comics           <NA>      good     NA
## 404             <NA>     NA         DC Comics           <NA>      good     NA
## 405             <NA>     NA         DC Comics           <NA>      good     NA
## 406          No Hair  193.0     Marvel Comics         silver      good    101
## 407            Black  157.0     Marvel Comics           <NA>      good     50
## 408            Black     NA         DC Comics           <NA>      good     NA
## 409            Black  198.0     Marvel Comics           <NA>      good    180
## 410            Blond  178.0     Marvel Comics           <NA>      good     49
## 411          No Hair     NA         DC Comics           <NA>      good     NA
## 412      Red / White  165.0     Marvel Comics           <NA>      good     65
## 413             <NA>  188.0         DC Comics           <NA>      good    113
## 414            Black  211.0      Image Comics           <NA>      good    405
## 415          No Hair     NA         DC Comics          white      good     NA
## 416             <NA>     NA     Marvel Comics           <NA>      good     NA
## 417             <NA>     NA         DC Comics           <NA>      good     NA
## 418            Brown     NA         DC Comics           <NA>      good     NA
## 419            Brown  170.0     Marvel Comics           <NA>      good     54
## 420            Blond  165.0     Marvel Comics           <NA>      good     56
## 421            Brown  178.0     Marvel Comics           <NA>      good     74
## 422            Brown  178.0     Marvel Comics           <NA>      good     77
## 423            Black  157.0     Marvel Comics           <NA>      good     56
## 424            Black  178.0     Marvel Comics           <NA>      good     59
## 425             <NA>     NA     Marvel Comics           <NA>      good     NA
## 426            Brown  173.0     Marvel Comics           <NA>      good     55
## 427            Black  185.0         Star Trek           <NA>      good     81
## 428            Blond  183.0     Marvel Comics           <NA>      good     83
## 429             <NA>     NA     Marvel Comics           <NA>      good     NA
## 430            Blond  188.0     Marvel Comics           <NA>      good     79
## 431             <NA>     NA     Marvel Comics           <NA>      good     NA
## 432           Auburn  193.0         DC Comics         orange      good     71
## 433            Blond  165.0         DC Comics           <NA>      good     62
## 434            Black  170.0         DC Comics           <NA>      good     63
## 435          No Hair  201.0         DC Comics           <NA>      good    131
## 436            Blond     NA       ABC Studios           <NA>      good     NA
## 437            White  180.0     Marvel Comics           <NA>      good     57
## 438            black  173.0     Marvel Comics           <NA>      good     77
## 439            Black  170.0         DC Comics           <NA>      good     68
## 440            Blond  165.0         DC Comics           <NA>      good     54
## 441            Black  191.0         DC Comics           <NA>      good    101
## 442            Black  180.0     Marvel Comics           <NA>      good     74
## 443            Black  163.0     Marvel Comics           <NA>      good     54
## 444             <NA>     NA              <NA>           <NA>      good     NA
## 445          No Hair  183.0     Marvel Comics           <NA>      good    225
## 446            Blond  198.0     Marvel Comics           <NA>      good    288
## 447            Blond  175.0     Marvel Comics           <NA>      good    143
## 448            Black  185.0     Marvel Comics           <NA>      good    101
## 449             <NA>     NA     Marvel Comics           <NA>      good     NA
## 450            Black  175.0     Marvel Comics           <NA>      good     74
## 451            Blond  198.0     Marvel Comics           <NA>      good    288
## 452              Red  218.0     Marvel Comics           <NA>      good    158
## 453           Auburn  178.0     Marvel Comics           <NA>      good     81
## 454             <NA>     NA     HarperCollins           <NA>      good     NA
## 455            Brown  188.0     Marvel Comics           <NA>      good     97
## 456            Blond  191.0     Marvel Comics           <NA>      good    117
## 457             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 458            Brown  168.0         DC Comics           <NA>      good     59
## 459          No Hair  188.0     Marvel Comics          green      good     86
## 460            Blond  168.0     Marvel Comics           <NA>      good    105
## 461 Strawberry Blond  168.0     Marvel Comics           <NA>      good     54
## 462            Black  175.0      Team Epic TV           <NA>      good     56
## 463            Blond  191.0     Marvel Comics           <NA>      good    214
## 464           Silver  168.0     Marvel Comics           <NA>      good     52
## 465            Black  178.0         DC Comics           <NA>      good     71
## 466              Red  165.0     Marvel Comics           <NA>      good     54
## 467             <NA>     NA     Marvel Comics           <NA>      good     NA
## 468            Black  137.0 Dark Horse Comics           <NA>      good     41
## 469          No Hair  191.0     Marvel Comics            red      good    135
## 470          No Hair  191.0     Marvel Comics           <NA>      good    135
## 471            Black  175.0         DC Comics           <NA>      good     63
## 472            Black     NA     Marvel Comics           <NA>      good     NA
## 473            Brown  185.0     Marvel Comics           <NA>      good     95
## 474            Blond  180.0     Marvel Comics           <NA>      good     54
## 475            Blond  188.0     Marvel Comics           <NA>      good    108
## 476            Black  218.0     Marvel Comics           <NA>      good    158
## 477           Auburn  163.0     Marvel Comics           <NA>      good     50
## 478             <NA>     NA     Marvel Comics           <NA>      good     NA
## 479            Blond  178.0     Marvel Comics           <NA>      good     65
## 480             <NA>     NA         DC Comics           <NA>      good     NA
## 481            Brown  175.0     Marvel Comics           <NA>      good    117
## 482            Black  140.0     Marvel Comics           <NA>      good     39
## 483           Auburn  366.0     Marvel Comics           <NA>      good    473
## 484            Black  160.0     Marvel Comics           <NA>      good    135
## 485            Blond  165.0         DC Comics           <NA>      good     51
## 486            Black  188.0     Marvel Comics           <NA>      good    171
## 487            Black  183.0         DC Comics           <NA>      good     74
## 488             <NA>     NA     Marvel Comics           <NA>      good     NA
## 489            Black  196.0     Marvel Comics           <NA>      good    117
## 490            Black  155.0     Marvel Comics           <NA>      good     50
## 491            Brown  175.0     Marvel Comics           <NA>      good     61
## 492            Blond  183.0     Marvel Comics           <NA>      good     83
## 493 Strawberry Blond  165.0     Marvel Comics           <NA>      good     52
## 494          No Hair  304.8     Marvel Comics          white      good     NA
## 495            White   66.0      George Lucas          green      good     17
## 496            Black  170.0         DC Comics           <NA>      good     57
```

```r
bad_guys <- data.frame(filter(superhero_info, alignment == "bad"))
bad_guys
```

```
##                  name gender               eye_color               race
## 1         Abomination   Male                   green  Human / Radiation
## 2             Abraxas   Male                    blue      Cosmic Entity
## 3       Absorbing Man   Male                    blue              Human
## 4          Air-Walker   Male                    blue               <NA>
## 5                Ajax   Male                   brown             Cyborg
## 6         Alex Mercer   Male                    <NA>              Human
## 7               Alien   Male                    <NA>    Xenomorph XX121
## 8               Amazo   Male                     red            Android
## 9                Ammo   Male                   brown              Human
## 10             Angela Female                    <NA>               <NA>
## 11          Annihilus   Male                   green               <NA>
## 12       Anti-Monitor   Male                  yellow      God / Eternal
## 13         Anti-Spawn   Male                    <NA>               <NA>
## 14         Apocalypse   Male                     red             Mutant
## 15           Arclight Female                  violet               <NA>
## 16              Atlas   Male                    blue      God / Eternal
## 17             Azazel   Male                  yellow           Neyaphem
## 18               Bane   Male                    <NA>              Human
## 19             Beetle   Male                    <NA>               <NA>
## 20          Big Barda Female                    blue            New God
## 21            Big Man   Male                    blue               <NA>
## 22      Billy Kincaid   Male                    <NA>               <NA>
## 23           Bird-Man   Male                    <NA>              Human
## 24        Bird-Man II   Male                    <NA>              Human
## 25       Black Abbott   Male                     red               <NA>
## 26         Black Adam   Male                   brown               <NA>
## 27        Black Mamba Female                   green               <NA>
## 28        Black Manta   Male                   black              Human
## 29           Blackout   Male                     red              Demon
## 30          Blackwing   Male                    blue               <NA>
## 31           Blizzard   Male                    <NA>               <NA>
## 32           Blizzard   Male                    <NA>               <NA>
## 33        Blizzard II   Male                   brown               <NA>
## 34               Blob   Male                   brown               <NA>
## 35           Bloodaxe Female                    blue              Human
## 36        Bloodwraith   Male                   white               <NA>
## 37          Boba Fett   Male                   brown      Human / Clone
## 38         Bomb Queen Female                    <NA>               <NA>
## 39           Brainiac   Male                   green            Android
## 40           Bullseye   Male                    blue              Human
## 41           Callisto Female                    blue               <NA>
## 42            Carnage   Male                   green           Symbiote
## 43          Chameleon   Male                    <NA>               <NA>
## 44         Changeling   Male                   brown               <NA>
## 45            Cheetah Female                   green              Human
## 46         Cheetah II Female                   green              Human
## 47        Cheetah III Female                   brown              Human
## 48            Chromos   Male                   brown               <NA>
## 49         Clock King   Male                    blue              Human
## 50         Cogliostro   Male                    <NA>               <NA>
## 51        Cottonmouth   Male                   brown              Human
## 52              Curse   Male                    <NA>               <NA>
## 53             Cy-Gor   Male                    <NA>               <NA>
## 54    Cyborg Superman   Male                    blue             Cyborg
## 55           Darkseid   Male                     red            New God
## 56           Darkside   <NA>                    <NA>               <NA>
## 57         Darth Maul   Male            yellow / red Dathomirian Zabrak
## 58        Darth Vader   Male                  yellow             Cyborg
## 59           Deadshot   Male                   brown              Human
## 60         Demogoblin   Male                     red              Demon
## 61          Destroyer   Male                    <NA>               <NA>
## 62        Diamondback   Male                   brown              Human
## 63        Doctor Doom   Male                   brown              Human
## 64     Doctor Doom II   Male                   brown               <NA>
## 65     Doctor Octopus   Male                   brown              Human
## 66           Doomsday   Male                     red              Alien
## 67       Doppelganger   Male                   white               <NA>
## 68           Dormammu   Male                  yellow               <NA>
## 69                Ego   <NA>                    <NA>               <NA>
## 70            Electro   Male                    blue              Human
## 71        Elle Bishop Female                    blue               <NA>
## 72      Evil Deadpool   Male                   white             Mutant
## 73           Evilhawk   Male                     red              Alien
## 74             Exodus   Male                    blue             Mutant
## 75      Fabian Cortez   <NA>                    blue               <NA>
## 76      Fallen One II   Male                   black               <NA>
## 77              Faora Female                    <NA>         Kryptonian
## 78              Fixer   <NA>                     red               <NA>
## 79             Frenzy Female                   brown               <NA>
## 80        General Zod   Male                   black         Kryptonian
## 81            Giganta Female                   green               <NA>
## 82       Goblin Queen Female                   green               <NA>
## 83           Godzilla   <NA>                    <NA>              Kaiju
## 84                Gog   Male                    <NA>               <NA>
## 85      Gorilla Grodd   Male                  yellow            Gorilla
## 86    Granny Goodness Female                    blue               <NA>
## 87             Greedo   Male                  purple             Rodian
## 88       Green Goblin   Male                    blue              Human
## 89    Green Goblin II   Male                    blue               <NA>
## 90       Harley Quinn Female                    blue              Human
## 91          Heat Wave   Male                    blue              Human
## 92               Hela Female                   green          Asgardian
## 93          Hobgoblin   Male                    blue               <NA>
## 94          Hydro-Man   Male                   brown               <NA>
## 95        Iron Monger   Male                    blue               <NA>
## 96             Jigsaw   Male                    blue               <NA>
## 97              Joker   Male                   green              Human
## 98           Junkpile   Male                    <NA>             Mutant
## 99               Kang   Male                   brown               <NA>
## 100       Killer Croc   Male                     red          Metahuman
## 101      Killer Frost Female                    blue              Human
## 102        King Shark   Male                   black             Animal
## 103           Kingpin   Male                    blue              Human
## 104              Klaw   Male                     red              Human
## 105         Kraven II   Male                   brown              Human
## 106 Kraven the Hunter   Male                   brown              Human
## 107          Kylo Ren   Male                    <NA>              Human
## 108     Lady Bullseye Female                    <NA>               <NA>
## 109  Lady Deathstrike Female                   brown             Cyborg
## 110            Leader   Male                   green               <NA>
## 111        Lex Luthor   Male                   green              Human
## 112    Lightning Lord   Male                    blue               <NA>
## 113      Living Brain   <NA>                  yellow               <NA>
## 114            Lizard   Male                     red              Human
## 115              Loki   Male                   green          Asgardian
## 116     Luke Campbell   Male                    <NA>               <NA>
## 117           Mach-IV   Male                   brown               <NA>
## 118           Magneto   Male                    grey             Mutant
## 119             Magus   Male                   black               <NA>
## 120          Mandarin   Male                    blue              Human
## 121             Match   Male                   black               <NA>
## 122            Maxima Female                   brown               <NA>
## 123          Mephisto   Male                   white               <NA>
## 124           Metallo   Male                   green            Android
## 125     Mister Freeze   Male                    <NA>              Human
## 126      Mister Knife   Male                    blue            Spartoi
## 127   Mister Mxyzptlk   Male                    <NA>      God / Eternal
## 128   Mister Sinister   Male                     red    Human / Altered
## 129      Mister Zsasz   Male                    blue              Human
## 130             MODOK   Male                   white             Cyborg
## 131            Moloch   Male                    <NA>               <NA>
## 132        Molten Man   Male                    gold               <NA>
## 133         Moonstone Female                    blue               <NA>
## 134            Morlun   Male             white / red               <NA>
## 135      Moses Magnum   Male                   brown               <NA>
## 136          Mysterio   Male                   brown              Human
## 137          Mystique Female yellow (without irises)             Mutant
## 138            Nebula Female                    blue          Luphomoid
## 139         Omega Red   Male                     red               <NA>
## 140         Onslaught   Male                     red             Mutant
## 141         Overtkill   Male                    <NA>               <NA>
## 142        Ozymandias   Male                    blue              Human
## 143         Parademon   <NA>                    <NA>          Parademon
## 144           Penguin   Male                    blue              Human
## 145          Plantman   Male                   green             Mutant
## 146         Plastique Female                    blue               <NA>
## 147        Poison Ivy Female                   green              Human
## 148          Predator   Male                    <NA>             Yautja
## 149    Professor Zoom   Male                    blue              Human
## 150      Proto-Goblin   Male                   green               <NA>
## 151        Purple Man   Male                  purple              Human
## 152              Pyro   Male                    blue               <NA>
## 153      Ra's Al Ghul   Male                   green              Human
## 154     Razor-Fist II   Male                    blue               <NA>
## 155          Red Mist   Male                    <NA>               <NA>
## 156         Red Skull   Male                    blue               <NA>
## 157       Redeemer II   Male                    <NA>               <NA>
## 158      Redeemer III   Male                    <NA>               <NA>
## 159             Rhino   Male                   brown  Human / Radiation
## 160         Rick Flag   Male                    blue               <NA>
## 161           Riddler   Male                    <NA>               <NA>
## 162        Sabretooth   Male                   amber             Mutant
## 163            Sauron   Male                    <NA>              Maiar
## 164         Scarecrow   Male                    blue              Human
## 165     Scarlet Witch Female                    blue             Mutant
## 166           Scorpia Female                   green               <NA>
## 167          Scorpion   Male                   brown              Human
## 168    Sebastian Shaw   Male                    <NA>             Mutant
## 169           Shocker   Male                   brown              Human
## 170             Siren Female                    blue          Atlantean
## 171          Siren II Female                   black               <NA>
## 172             Siryn Female                    blue               <NA>
## 173        Snake-Eyes   Male                    <NA>             Animal
## 174    Solomon Grundy   Male                   black             Zombie
## 175    Spider-Carnage   Male                    <NA>           Symbiote
## 176   Spider-Woman IV Female                     red               <NA>
## 177       Steppenwolf   Male                     red            New God
## 178      Stormtrooper   Male                    <NA>              Human
## 179    Superboy-Prime   Male                    blue         Kryptonian
## 180       Swamp Thing   Male                     red      God / Eternal
## 181             Swarm   Male                  yellow             Mutant
## 182             Sylar   Male                    <NA>               <NA>
## 183            T-1000   Male                    <NA>            Android
## 184             T-800   Male                     red             Cyborg
## 185             T-850   Male                     red             Cyborg
## 186               T-X Female                    <NA>             Cyborg
## 187        Taskmaster   Male                   brown              Human
## 188            Thanos   Male                     red            Eternal
## 189       Tiger Shark   Male                    grey              Human
## 190          Tinkerer   Male                   brown               <NA>
## 191            Trigon   Male                  yellow      God / Eternal
## 192          Two-Face   Male                    <NA>               <NA>
## 193            Ultron   Male                     red            Android
## 194       Utgard-Loki   Male                    blue        Frost Giant
## 195          Vanisher   Male                   green               <NA>
## 196            Vegeta   Male                    <NA>             Saiyan
## 197             Venom   Male                    blue           Symbiote
## 198          Venom II   Male                   brown               <NA>
## 199         Venom III   Male                   brown           Symbiote
## 200          Violator   Male                    <NA>               <NA>
## 201           Vulture   Male                   brown              Human
## 202            Walrus   Male                    blue              Human
## 203              Warp   Male                   brown               <NA>
## 204         Weapon XI   Male                    <NA>               <NA>
## 205      White Canary Female                   brown              Human
## 206       Yellow Claw   Male                    blue               <NA>
## 207              Zoom   Male                     red               <NA>
##           hair_color height         publisher  skin_color alignment weight
## 1            No Hair  203.0     Marvel Comics        <NA>       bad    441
## 2              Black     NA     Marvel Comics        <NA>       bad     NA
## 3            No Hair  193.0     Marvel Comics        <NA>       bad    122
## 4              White  188.0     Marvel Comics        <NA>       bad    108
## 5              Black  193.0     Marvel Comics        <NA>       bad     90
## 6               <NA>     NA         Wildstorm        <NA>       bad     NA
## 7            No Hair  244.0 Dark Horse Comics       black       bad    169
## 8               <NA>  257.0         DC Comics        <NA>       bad    173
## 9              Black  188.0     Marvel Comics        <NA>       bad    101
## 10              <NA>     NA      Image Comics        <NA>       bad     NA
## 11           No Hair  180.0     Marvel Comics        <NA>       bad     90
## 12           No Hair   61.0         DC Comics        <NA>       bad     NA
## 13              <NA>     NA      Image Comics        <NA>       bad     NA
## 14             Black  213.0     Marvel Comics        grey       bad    135
## 15            Purple  173.0     Marvel Comics        <NA>       bad     57
## 16             Brown  198.0         DC Comics        <NA>       bad    126
## 17             Black  183.0     Marvel Comics         red       bad     67
## 18              <NA>  203.0         DC Comics        <NA>       bad    180
## 19              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 20             Black  188.0         DC Comics        <NA>       bad    135
## 21             Brown  165.0     Marvel Comics        <NA>       bad     71
## 22              <NA>     NA      Image Comics        <NA>       bad     NA
## 23              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 24              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 25             Black     NA     Marvel Comics        <NA>       bad     NA
## 26             Black  191.0         DC Comics        <NA>       bad    113
## 27             Black  170.0     Marvel Comics        <NA>       bad     52
## 28           No Hair  188.0         DC Comics        <NA>       bad     92
## 29             White  191.0     Marvel Comics       white       bad    104
## 30             Black  185.0     Marvel Comics        <NA>       bad     86
## 31              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 32             Brown     NA     Marvel Comics        <NA>       bad     NA
## 33             Brown  175.0     Marvel Comics        <NA>       bad     77
## 34             Brown  178.0     Marvel Comics        <NA>       bad    230
## 35             Brown  218.0     Marvel Comics        <NA>       bad    495
## 36           No Hair   30.5     Marvel Comics        <NA>       bad     NA
## 37             Black  183.0      George Lucas        <NA>       bad     NA
## 38              <NA>     NA      Image Comics        <NA>       bad     NA
## 39           No Hair  198.0         DC Comics       green       bad    135
## 40             blond  183.0     Marvel Comics        <NA>       bad     90
## 41             Black  175.0     Marvel Comics        <NA>       bad     74
## 42               Red  185.0     Marvel Comics        <NA>       bad     86
## 43              <NA>     NA         DC Comics        <NA>       bad     NA
## 44             Black  180.0     Marvel Comics        <NA>       bad     81
## 45             Blond  163.0         DC Comics        <NA>       bad     50
## 46             Brown  170.0         DC Comics        <NA>       bad     55
## 47             Brown  175.0         DC Comics        <NA>       bad     54
## 48        Red / Grey  185.0      Team Epic TV        <NA>       bad     86
## 49             Black  178.0         DC Comics        <NA>       bad     78
## 50              <NA>     NA      Image Comics        <NA>       bad     NA
## 51             Black  183.0     Marvel Comics        <NA>       bad     99
## 52              <NA>     NA      Image Comics        <NA>       bad     NA
## 53              <NA>     NA      Image Comics        <NA>       bad     NA
## 54             Black     NA         DC Comics        <NA>       bad     NA
## 55           No Hair  267.0         DC Comics        grey       bad    817
## 56              <NA>     NA              <NA>        <NA>       bad     NA
## 57              <NA>  170.0      George Lucas red / black       bad     NA
## 58           No Hair  198.0      George Lucas        <NA>       bad    135
## 59             Brown  185.0         DC Comics        <NA>       bad     91
## 60           No Hair  185.0     Marvel Comics        <NA>       bad     95
## 61              <NA>  188.0     Marvel Comics        <NA>       bad    383
## 62             Black  193.0     Marvel Comics        <NA>       bad     90
## 63             Brown  201.0     Marvel Comics        <NA>       bad    187
## 64             Brown  201.0     Marvel Comics        <NA>       bad    132
## 65             Brown  175.0     Marvel Comics        <NA>       bad    110
## 66             White  244.0         DC Comics        <NA>       bad    412
## 67           No Hair  196.0     Marvel Comics        <NA>       bad    104
## 68           No Hair  185.0     Marvel Comics        <NA>       bad     NA
## 69              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 70            Auburn  180.0     Marvel Comics        <NA>       bad     74
## 71             Blond     NA      NBC - Heroes        <NA>       bad     NA
## 72               Red  188.0     Marvel Comics        <NA>       bad     95
## 73             Black  191.0     Marvel Comics       green       bad    106
## 74             Black  183.0     Marvel Comics         red       bad     88
## 75             Brown  196.0     Marvel Comics        <NA>       bad     96
## 76              Blue     NA     Marvel Comics        <NA>       bad     NA
## 77              <NA>     NA         DC Comics        <NA>       bad     NA
## 78           No Hair     NA     Marvel Comics        <NA>       bad     NA
## 79             Black  211.0     Marvel Comics        <NA>       bad    104
## 80             Black     NA         DC Comics        <NA>       bad     NA
## 81               Red   62.5         DC Comics        <NA>       bad    630
## 82               Red  168.0     Marvel Comics        <NA>       bad     50
## 83              <NA>  108.0              <NA>        grey       bad     NA
## 84              <NA>     NA         DC Comics        <NA>       bad     NA
## 85             Black  198.0         DC Comics        <NA>       bad    270
## 86             White  178.0         DC Comics        <NA>       bad    115
## 87              <NA>  170.0      George Lucas       green       bad     NA
## 88            Auburn  180.0     Marvel Comics        <NA>       bad     83
## 89            Auburn  178.0     Marvel Comics        <NA>       bad     77
## 90             Blond  170.0         DC Comics        <NA>       bad     63
## 91           No Hair  180.0         DC Comics        <NA>       bad     81
## 92             Black  213.0     Marvel Comics        <NA>       bad    225
## 93              Grey  180.0     Marvel Comics        <NA>       bad     83
## 94             Brown  188.0     Marvel Comics        <NA>       bad    119
## 95           No Hair     NA     Marvel Comics        <NA>       bad      2
## 96             Black  188.0     Marvel Comics        <NA>       bad    113
## 97             Green  196.0         DC Comics       white       bad     86
## 98              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 99             Brown  191.0     Marvel Comics        <NA>       bad    104
## 100          No Hair  244.0         DC Comics       green       bad    356
## 101            Blond     NA         DC Comics        blue       bad     NA
## 102          No Hair     NA         DC Comics        <NA>       bad     NA
## 103          No Hair  201.0     Marvel Comics        <NA>       bad    203
## 104          No Hair  188.0     Marvel Comics         red       bad     97
## 105            Black  191.0     Marvel Comics        <NA>       bad     99
## 106            Black  183.0     Marvel Comics        <NA>       bad    106
## 107             <NA>     NA      George Lucas        <NA>       bad     NA
## 108            Black     NA     Marvel Comics        <NA>       bad     NA
## 109            Black  175.0     Marvel Comics        <NA>       bad     58
## 110          No Hair  178.0     Marvel Comics        <NA>       bad     63
## 111          No Hair  188.0         DC Comics        <NA>       bad     95
## 112              Red  191.0         DC Comics        <NA>       bad     95
## 113             <NA>  198.0     Marvel Comics        <NA>       bad    360
## 114          No Hair  203.0     Marvel Comics        <NA>       bad    230
## 115            Black  193.0     Marvel Comics        <NA>       bad    236
## 116             <NA>     NA      NBC - Heroes        <NA>       bad     NA
## 117            Brown  180.0     Marvel Comics        <NA>       bad     79
## 118            White  188.0     Marvel Comics        <NA>       bad     86
## 119             <NA>  183.0     Marvel Comics        <NA>       bad     NA
## 120            White  188.0     Marvel Comics        <NA>       bad     97
## 121            Black     NA         DC Comics        <NA>       bad     NA
## 122              Red  180.0         DC Comics        <NA>       bad     72
## 123            Black  198.0     Marvel Comics        <NA>       bad    140
## 124            Brown  196.0         DC Comics        <NA>       bad     90
## 125             <NA>  183.0         DC Comics        <NA>       bad     86
## 126            Brown     NA     Marvel Comics        <NA>       bad     NA
## 127             <NA>     NA         DC Comics        <NA>       bad     NA
## 128            Black  196.0     Marvel Comics        <NA>       bad    128
## 129            Blond     NA         DC Comics        <NA>       bad     NA
## 130           Brownn  366.0     Marvel Comics        <NA>       bad    338
## 131             <NA>     NA         DC Comics        <NA>       bad     NA
## 132             Gold  196.0     Marvel Comics        <NA>       bad    248
## 133            Blond  180.0     Marvel Comics        <NA>       bad     59
## 134            Black  188.0     Marvel Comics        <NA>       bad     79
## 135            Black  175.0     Marvel Comics        <NA>       bad     72
## 136          No Hair  180.0     Marvel Comics        <NA>       bad     79
## 137     Red / Orange  178.0     Marvel Comics        blue       bad     54
## 138          No Hair  185.0     Marvel Comics        blue       bad     83
## 139            Blond  211.0     Marvel Comics        <NA>       bad    191
## 140          No Hair  305.0     Marvel Comics        <NA>       bad    405
## 141             <NA>     NA      Image Comics        <NA>       bad     NA
## 142            Blond     NA         DC Comics        <NA>       bad     NA
## 143             <NA>     NA         DC Comics        <NA>       bad     NA
## 144            Black  157.0         DC Comics        <NA>       bad     79
## 145             Grey  183.0     Marvel Comics        <NA>       bad     87
## 146              Red  168.0         DC Comics        <NA>       bad     55
## 147              Red  168.0         DC Comics       green       bad     50
## 148             <NA>  213.0 Dark Horse Comics        <NA>       bad    234
## 149 Strawberry Blond  180.0         DC Comics        <NA>       bad     81
## 150            Blond     NA     Marvel Comics        <NA>       bad     NA
## 151           Purple  180.0     Marvel Comics      purple       bad     74
## 152            Blond  178.0     Marvel Comics        <NA>       bad     68
## 153             Grey  193.0         DC Comics        <NA>       bad     97
## 154          No Hair  191.0     Marvel Comics        <NA>       bad    117
## 155             <NA>     NA       Icon Comics        <NA>       bad     NA
## 156          No Hair  188.0     Marvel Comics        <NA>       bad    108
## 157             <NA>     NA      Image Comics        <NA>       bad     NA
## 158             <NA>     NA      Image Comics        <NA>       bad     NA
## 159            Brown  196.0     Marvel Comics        <NA>       bad    320
## 160            Brown  185.0         DC Comics        <NA>       bad     85
## 161             <NA>     NA         DC Comics        <NA>       bad     NA
## 162            Blond  198.0     Marvel Comics        <NA>       bad    171
## 163             <NA>  279.0  J. R. R. Tolkien        <NA>       bad     NA
## 164            Brown  183.0         DC Comics        <NA>       bad     63
## 165            Brown  170.0     Marvel Comics        <NA>       bad     59
## 166              Red     NA     Marvel Comics        <NA>       bad     NA
## 167            Brown  211.0     Marvel Comics        <NA>       bad    310
## 168             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 169            Brown  175.0     Marvel Comics        <NA>       bad     79
## 170           Purple  175.0         DC Comics        <NA>       bad     72
## 171             <NA>     NA         DC Comics        <NA>       bad     NA
## 172 Strawberry Blond  168.0     Marvel Comics        <NA>       bad     52
## 173             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 174            White  279.0         DC Comics        <NA>       bad    437
## 175             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 176            White  178.0     Marvel Comics        <NA>       bad     58
## 177            Black  183.0         DC Comics       white       bad     91
## 178             <NA>  183.0      George Lucas        <NA>       bad     NA
## 179     Black / Blue  180.0         DC Comics        <NA>       bad     77
## 180          No Hair     NA         DC Comics       green       bad     NA
## 181          No Hair  196.0     Marvel Comics      yellow       bad     47
## 182             <NA>     NA      NBC - Heroes        <NA>       bad     NA
## 183             <NA>  183.0 Dark Horse Comics      silver       bad    146
## 184             <NA>     NA Dark Horse Comics        <NA>       bad    176
## 185             <NA>     NA Dark Horse Comics        <NA>       bad    198
## 186             <NA>     NA Dark Horse Comics      silver       bad    149
## 187            Brown  188.0     Marvel Comics        <NA>       bad     99
## 188          No Hair  201.0     Marvel Comics      purple       bad    443
## 189          No Hair  185.0     Marvel Comics        grey       bad    203
## 190            White  163.0     Marvel Comics        <NA>       bad     54
## 191            Black     NA         DC Comics         red       bad     NA
## 192             <NA>  183.0         DC Comics        <NA>       bad     82
## 193             <NA>  206.0     Marvel Comics      silver       bad    331
## 194            White   15.2     Marvel Comics        <NA>       bad     58
## 195          No Hair  165.0     Marvel Comics        <NA>       bad     79
## 196            Black  168.0          Shueisha        <NA>       bad     73
## 197 Strawberry Blond  191.0     Marvel Comics        <NA>       bad    117
## 198            Black  175.0     Marvel Comics        <NA>       bad     50
## 199            Brown  229.0     Marvel Comics        <NA>       bad    334
## 200             <NA>     NA      Image Comics        <NA>       bad     NA
## 201          No Hair  180.0     Marvel Comics        <NA>       bad     79
## 202            Black  183.0     Marvel Comics        <NA>       bad    162
## 203            Black  173.0         DC Comics        <NA>       bad     67
## 204             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 205            Black     NA         DC Comics        <NA>       bad     NA
## 206          No Hair  188.0     Marvel Comics        <NA>       bad     95
## 207            Brown  185.0         DC Comics        <NA>       bad     81
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guys %>%
  filter(race == "Asgardian")
```

```
##        name gender eye_color      race hair_color height     publisher
## 1       Sif Female      blue Asgardian      Black    188 Marvel Comics
## 2      Thor   Male      blue Asgardian      Blond    198 Marvel Comics
## 3 Thor Girl Female      blue Asgardian      Blond    175 Marvel Comics
##   skin_color alignment weight
## 1       <NA>      good    191
## 2       <NA>      good    288
## 3       <NA>      good    143
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guys %>%
  filter(gender == "Male" & height > 200)
```

```
##              name gender eye_color              race hair_color height
## 1     Abomination   Male     green Human / Radiation    No Hair    203
## 2           Alien   Male      <NA>   Xenomorph XX121    No Hair    244
## 3           Amazo   Male       red           Android       <NA>    257
## 4      Apocalypse   Male       red            Mutant      Black    213
## 5            Bane   Male      <NA>             Human       <NA>    203
## 6        Darkseid   Male       red           New God    No Hair    267
## 7     Doctor Doom   Male     brown             Human      Brown    201
## 8  Doctor Doom II   Male     brown              <NA>      Brown    201
## 9        Doomsday   Male       red             Alien      White    244
## 10    Killer Croc   Male       red         Metahuman    No Hair    244
## 11        Kingpin   Male      blue             Human    No Hair    201
## 12         Lizard   Male       red             Human    No Hair    203
## 13          MODOK   Male     white            Cyborg     Brownn    366
## 14      Omega Red   Male       red              <NA>      Blond    211
## 15      Onslaught   Male       red            Mutant    No Hair    305
## 16       Predator   Male      <NA>            Yautja       <NA>    213
## 17         Sauron   Male      <NA>             Maiar       <NA>    279
## 18       Scorpion   Male     brown             Human      Brown    211
## 19 Solomon Grundy   Male     black            Zombie      White    279
## 20         Thanos   Male       red           Eternal    No Hair    201
## 21         Ultron   Male       red           Android       <NA>    206
## 22      Venom III   Male     brown          Symbiote      Brown    229
##            publisher skin_color alignment weight
## 1      Marvel Comics       <NA>       bad    441
## 2  Dark Horse Comics      black       bad    169
## 3          DC Comics       <NA>       bad    173
## 4      Marvel Comics       grey       bad    135
## 5          DC Comics       <NA>       bad    180
## 6          DC Comics       grey       bad    817
## 7      Marvel Comics       <NA>       bad    187
## 8      Marvel Comics       <NA>       bad    132
## 9          DC Comics       <NA>       bad    412
## 10         DC Comics      green       bad    356
## 11     Marvel Comics       <NA>       bad    203
## 12     Marvel Comics       <NA>       bad    230
## 13     Marvel Comics       <NA>       bad    338
## 14     Marvel Comics       <NA>       bad    191
## 15     Marvel Comics       <NA>       bad    405
## 16 Dark Horse Comics       <NA>       bad    234
## 17  J. R. R. Tolkien       <NA>       bad     NA
## 18     Marvel Comics       <NA>       bad    310
## 19         DC Comics       <NA>       bad    437
## 20     Marvel Comics     purple       bad    443
## 21     Marvel Comics     silver       bad    331
## 22     Marvel Comics       <NA>       bad    334
```
</div>

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
good_guys %>%
  filter(hair_color == "No Hair")
```

```
##                  name gender eye_color              race hair_color height
## 1              A-Bomb   Male    yellow             Human    No Hair  203.0
## 2          Abe Sapien   Male      blue     Icthyo Sapien    No Hair  191.0
## 3            Abin Sur   Male      blue           Ungaran    No Hair  185.0
## 4       Beta Ray Bill   Male      <NA>              <NA>    No Hair  201.0
## 5              Bishop   Male     brown            Mutant    No Hair  198.0
## 6     Black Lightning   Male     brown              <NA>    No Hair  185.0
## 7         Blaquesmith   <NA>     black              <NA>    No Hair     NA
## 8           Bloodhawk   Male     black            Mutant    No Hair     NA
## 9      Crimson Dynamo   Male     brown              <NA>    No Hair  180.0
## 10          Donatello   Male     green            Mutant    No Hair     NA
## 11       Dr Manhattan   Male     white    Human / Cosmic    No Hair     NA
## 12 Drax the Destroyer   Male       red   Human / Altered    No Hair  193.0
## 13      Fin Fang Foom   Male       red   Kakarantharaian    No Hair  975.0
## 14        Ghost Rider   Male       red             Demon    No Hair  188.0
## 15                Ink   Male      blue            Mutant    No Hair  180.0
## 16              K-2SO   Male     white           Android    No Hair  213.0
## 17            Kilowog   Male       red        Bolovaxian    No Hair  234.0
## 18       Kool-Aid Man   Male     black              <NA>    No Hair     NA
## 19           Leonardo   Male      blue            Mutant    No Hair     NA
## 20          Man-Thing   Male       red              <NA>    No Hair  213.0
## 21  Martian Manhunter   Male       red           Martian    No Hair  201.0
## 22         Metamorpho   Male     black              <NA>    No Hair  185.0
## 23              Morph   Male     white              <NA>    No Hair  178.0
## 24      One Punch Man   Male      <NA>             Human    No Hair  175.0
## 25        Professor X   Male      blue            Mutant    No Hair  183.0
## 26            Raphael   Male      <NA>            Mutant    No Hair     NA
## 27        Red Tornado   Male     green           Android    No Hair  185.0
## 28          She-Thing Female      blue Human / Radiation    No Hair  183.0
## 29      Silver Surfer   Male     white             Alien    No Hair  193.0
## 30              Sobek   Male     white              <NA>    No Hair     NA
## 31            Spectre   Male     white     God / Eternal    No Hair     NA
## 32              Steel   Male     brown              <NA>    No Hair  201.0
## 33              Thing   Male      blue Human / Radiation    No Hair  183.0
## 34             Triton   Male     green           Inhuman    No Hair  188.0
## 35             Vision   Male      gold           Android    No Hair  191.0
## 36          Vision II   <NA>       red              <NA>    No Hair  191.0
## 37               Ymir   Male     white       Frost Giant    No Hair  304.8
##            publisher skin_color alignment weight
## 1      Marvel Comics       <NA>      good    441
## 2  Dark Horse Comics       blue      good     65
## 3          DC Comics        red      good     90
## 4      Marvel Comics       <NA>      good    216
## 5      Marvel Comics       <NA>      good    124
## 6          DC Comics       <NA>      good     90
## 7      Marvel Comics       <NA>      good     NA
## 8      Marvel Comics       <NA>      good     NA
## 9      Marvel Comics       <NA>      good    104
## 10    IDW Publishing      green      good     NA
## 11         DC Comics       blue      good     NA
## 12     Marvel Comics      green      good    306
## 13     Marvel Comics      green      good     18
## 14     Marvel Comics       <NA>      good     99
## 15     Marvel Comics       <NA>      good     81
## 16      George Lucas       gray      good     NA
## 17         DC Comics       pink      good    324
## 18              <NA>        red      good     NA
## 19    IDW Publishing      green      good     NA
## 20     Marvel Comics      green      good    225
## 21         DC Comics      green      good    135
## 22         DC Comics       <NA>      good     90
## 23     Marvel Comics       <NA>      good     79
## 24          Shueisha       <NA>      good     69
## 25     Marvel Comics       <NA>      good     86
## 26    IDW Publishing      green      good     NA
## 27         DC Comics       <NA>      good    146
## 28     Marvel Comics       <NA>      good    153
## 29     Marvel Comics     silver      good    101
## 30         DC Comics       <NA>      good     NA
## 31         DC Comics      white      good     NA
## 32         DC Comics       <NA>      good    131
## 33     Marvel Comics       <NA>      good    225
## 34     Marvel Comics      green      good     86
## 35     Marvel Comics        red      good    135
## 36     Marvel Comics       <NA>      good    135
## 37     Marvel Comics      white      good     NA
```

```r
bad_guys %>%
  filter(hair_color == "No Hair")
```

```
##             name gender eye_color              race hair_color height
## 1    Abomination   Male     green Human / Radiation    No Hair  203.0
## 2  Absorbing Man   Male      blue             Human    No Hair  193.0
## 3          Alien   Male      <NA>   Xenomorph XX121    No Hair  244.0
## 4      Annihilus   Male     green              <NA>    No Hair  180.0
## 5   Anti-Monitor   Male    yellow     God / Eternal    No Hair   61.0
## 6    Black Manta   Male     black             Human    No Hair  188.0
## 7    Bloodwraith   Male     white              <NA>    No Hair   30.5
## 8       Brainiac   Male     green           Android    No Hair  198.0
## 9       Darkseid   Male       red           New God    No Hair  267.0
## 10   Darth Vader   Male    yellow            Cyborg    No Hair  198.0
## 11    Demogoblin   Male       red             Demon    No Hair  185.0
## 12  Doppelganger   Male     white              <NA>    No Hair  196.0
## 13      Dormammu   Male    yellow              <NA>    No Hair  185.0
## 14         Fixer   <NA>       red              <NA>    No Hair     NA
## 15     Heat Wave   Male      blue             Human    No Hair  180.0
## 16   Iron Monger   Male      blue              <NA>    No Hair     NA
## 17   Killer Croc   Male       red         Metahuman    No Hair  244.0
## 18    King Shark   Male     black            Animal    No Hair     NA
## 19       Kingpin   Male      blue             Human    No Hair  201.0
## 20          Klaw   Male       red             Human    No Hair  188.0
## 21        Leader   Male     green              <NA>    No Hair  178.0
## 22    Lex Luthor   Male     green             Human    No Hair  188.0
## 23        Lizard   Male       red             Human    No Hair  203.0
## 24      Mysterio   Male     brown             Human    No Hair  180.0
## 25        Nebula Female      blue         Luphomoid    No Hair  185.0
## 26     Onslaught   Male       red            Mutant    No Hair  305.0
## 27 Razor-Fist II   Male      blue              <NA>    No Hair  191.0
## 28     Red Skull   Male      blue              <NA>    No Hair  188.0
## 29   Swamp Thing   Male       red     God / Eternal    No Hair     NA
## 30         Swarm   Male    yellow            Mutant    No Hair  196.0
## 31        Thanos   Male       red           Eternal    No Hair  201.0
## 32   Tiger Shark   Male      grey             Human    No Hair  185.0
## 33      Vanisher   Male     green              <NA>    No Hair  165.0
## 34       Vulture   Male     brown             Human    No Hair  180.0
## 35   Yellow Claw   Male      blue              <NA>    No Hair  188.0
##            publisher skin_color alignment weight
## 1      Marvel Comics       <NA>       bad    441
## 2      Marvel Comics       <NA>       bad    122
## 3  Dark Horse Comics      black       bad    169
## 4      Marvel Comics       <NA>       bad     90
## 5          DC Comics       <NA>       bad     NA
## 6          DC Comics       <NA>       bad     92
## 7      Marvel Comics       <NA>       bad     NA
## 8          DC Comics      green       bad    135
## 9          DC Comics       grey       bad    817
## 10      George Lucas       <NA>       bad    135
## 11     Marvel Comics       <NA>       bad     95
## 12     Marvel Comics       <NA>       bad    104
## 13     Marvel Comics       <NA>       bad     NA
## 14     Marvel Comics       <NA>       bad     NA
## 15         DC Comics       <NA>       bad     81
## 16     Marvel Comics       <NA>       bad      2
## 17         DC Comics      green       bad    356
## 18         DC Comics       <NA>       bad     NA
## 19     Marvel Comics       <NA>       bad    203
## 20     Marvel Comics        red       bad     97
## 21     Marvel Comics       <NA>       bad     63
## 22         DC Comics       <NA>       bad     95
## 23     Marvel Comics       <NA>       bad    230
## 24     Marvel Comics       <NA>       bad     79
## 25     Marvel Comics       blue       bad     83
## 26     Marvel Comics       <NA>       bad    405
## 27     Marvel Comics       <NA>       bad    117
## 28     Marvel Comics       <NA>       bad    108
## 29         DC Comics      green       bad     NA
## 30     Marvel Comics     yellow       bad     47
## 31     Marvel Comics     purple       bad    443
## 32     Marvel Comics       grey       bad    203
## 33     Marvel Comics       <NA>       bad     79
## 34     Marvel Comics       <NA>       bad     79
## 35     Marvel Comics       <NA>       bad     95
```
There are more good guys that are bald.

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 300 or weight over 450?

```r
superhero_info %>%
  filter(height > 300 | weight > 450)
```

```
## # A tibble: 14 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
##  2 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
##  3 Fin ~ Male   red       Kaka~ No Hair     975   Marvel C~ green      good     
##  4 Gala~ Male   black     Cosm~ Black       876   Marvel C~ <NA>       neutral  
##  5 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  6 Groot Male   yellow    Flor~ <NA>        701   Marvel C~ <NA>       good     
##  7 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
##  8 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
##  9 MODOK Male   white     Cybo~ Brownn      366   Marvel C~ <NA>       bad      
## 10 Onsl~ Male   red       Muta~ No Hair     305   Marvel C~ <NA>       bad      
## 11 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 12 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 13 Wolf~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## 14 Ymir  Male   white     Fros~ No Hair     305.  Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>%
  filter(height > 300)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin ~ Male   red       Kaka~ No Hair      975  Marvel C~ green      good     
## 2 Gala~ Male   black     Cosm~ Black        876  Marvel C~ <NA>       neutral  
## 3 Groot Male   yellow    Flor~ <NA>         701  Marvel C~ <NA>       good     
## 4 MODOK Male   white     Cybo~ Brownn       366  Marvel C~ <NA>       bad      
## 5 Onsl~ Male   red       Muta~ No Hair      305  Marvel C~ <NA>       bad      
## 6 Sasq~ Male   red       <NA>  Orange       305  Marvel C~ <NA>       good     
## 7 Wolf~ Female green     <NA>  Auburn       366  Marvel C~ <NA>       good     
## 8 Ymir  Male   white     Fros~ No Hair      305. Marvel C~ white      good     
## # ... with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>%
  filter(weight > 450)
```

```
## # A tibble: 8 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
## 2 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
## 3 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
## 5 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
## 6 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
## 7 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
## 8 Wolf~ Female green     <NA>  Auburn      366   Marvel C~ <NA>       good     
## # ... with 1 more variable: weight <dbl>
```
We have less than 16 rows in question #10 because there are superheroes whose both over 300 in height and over 450 in weight. 

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the lowest height to weight ratio?

```r
superhero_info %>%
  mutate(height_weight_ratio = height/weight) %>%
  arrange(height_weight_ratio)
```

```
## # A tibble: 734 x 11
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 Giga~ Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
##  2 Utga~ Male   blue      Fros~ White        15.2 Marvel C~ <NA>       bad      
##  3 Dark~ Male   red       New ~ No Hair     267   DC Comics grey       bad      
##  4 Jugg~ Male   blue      Human Red         287   Marvel C~ <NA>       neutral  
##  5 Red ~ Male   yellow    Huma~ Black       213   Marvel C~ red        neutral  
##  6 Sasq~ Male   red       <NA>  Orange      305   Marvel C~ <NA>       good     
##  7 Hulk  Male   green     Huma~ Green       244   Marvel C~ green      good     
##  8 Bloo~ Female blue      Human Brown       218   Marvel C~ <NA>       bad      
##  9 Than~ Male   red       Eter~ No Hair     201   Marvel C~ purple     bad      
## 10 A-Bo~ Male   yellow    Human No Hair     203   Marvel C~ <NA>       good     
## # ... with 724 more rows, and 2 more variables: weight <dbl>,
## #   height_weight_ratio <dbl>
```
Giganta, Utgard-Loki, and Darkseid etc. have the lowest height to weight ratio.

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Ab...
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE...
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE,...
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALS...
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE,...
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, T...
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, ...
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE,...
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE...
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE...
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>%
  filter(accelerated_healing == "TRUE" & durability == "TRUE" & super_strength == "TRUE")
```

```
## # A tibble: 97 x 168
##    hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##    <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
##  1 A-Bomb     FALSE   TRUE             FALSE            FALSE           
##  2 Abe Sapien TRUE    TRUE             FALSE            FALSE           
##  3 Angel      TRUE    TRUE             FALSE            FALSE           
##  4 Anti-Moni~ FALSE   TRUE             FALSE            TRUE            
##  5 Anti-Venom FALSE   TRUE             FALSE            FALSE           
##  6 Aquaman    TRUE    TRUE             FALSE            FALSE           
##  7 Arachne    TRUE    TRUE             FALSE            FALSE           
##  8 Archangel  TRUE    TRUE             FALSE            FALSE           
##  9 Ardina     TRUE    TRUE             FALSE            FALSE           
## 10 Ares       TRUE    TRUE             FALSE            FALSE           
## # ... with 87 more rows, and 163 more variables: cold_resistance <lgl>,
## #   durability <lgl>, stealth <lgl>, energy_absorption <lgl>, flight <lgl>,
## #   danger_sense <lgl>, underwater_breathing <lgl>, marksmanship <lgl>,
## #   weapons_master <lgl>, power_augmentation <lgl>, animal_attributes <lgl>,
## #   longevity <lgl>, intelligence <lgl>, super_strength <lgl>,
## #   cryokinesis <lgl>, telepathy <lgl>, energy_armor <lgl>,
## #   energy_blasts <lgl>, duplication <lgl>, size_changing <lgl>,
## #   density_control <lgl>, stamina <lgl>, astral_travel <lgl>,
## #   audio_control <lgl>, dexterity <lgl>, omnitrix <lgl>, super_speed <lgl>,
## #   possession <lgl>, animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```
97 superheros have a combination of accelerated healing, durability, and super strength.

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
superhero_powers %>%
  select(hero_names, ends_with("kinesis"))%>%
  filter_all(any_vars(. == "TRUE"))
```

```
## # A tibble: 112 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 Alan Scott FALSE       FALSE          FALSE       FALSE        TRUE        
##  2 Amazo      TRUE        FALSE          FALSE       FALSE        FALSE       
##  3 Apocalypse FALSE       FALSE          TRUE        FALSE        FALSE       
##  4 Aqualad    TRUE        FALSE          FALSE       FALSE        FALSE       
##  5 Beyonder   FALSE       FALSE          TRUE        FALSE        FALSE       
##  6 Bizarro    TRUE        FALSE          FALSE       FALSE        TRUE        
##  7 Black Abb~ FALSE       FALSE          TRUE        FALSE        FALSE       
##  8 Black Adam FALSE       FALSE          TRUE        FALSE        FALSE       
##  9 Black Lig~ FALSE       TRUE           FALSE       FALSE        FALSE       
## 10 Black Mam~ FALSE       FALSE          FALSE       FALSE        TRUE        
## # ... with 102 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>%
  filter(hero_names == "Iron Man")
```

```
## # A tibble: 1 x 168
##   hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 Iron Man   FALSE   TRUE             FALSE            FALSE           
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, ...
```
My favorite superhero is Iron Man, he has accelerated healing, durability, energy absorption, flight and a lot more powers.
