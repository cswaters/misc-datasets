# misc-datasets
Random small-ish datasets that might be of use to someone. I don't remember the original source for most of these.


## Notes

- `pregame.csv` is based on raw data from the article, [How America’s Favorite Sports Betting Expert Turned A Sucker’s Game Into An Industry](https://deadspin.com/how-america-s-favorite-sports-betting-expert-turned-a-s-1782438574) in Deadspin. The source file is an `xlsx` file. I cleaned it and dropped some rows. The data consists of 106,923 records. The code is below.

```r

# filter for touts with 200+ bets
n_200 <- pregame %>% 
  count(tout=tolower(tout)) %>% 
  filter(n >= 200) %>% 
  pull(tout)

pregame %>% 
  filter(`w/l` %in% c('W','L','T'),
         tolower(tout) %in% n_200,
         !is.na(net),
         !is.na(sport),
         sport!='Fees') %>% 
  transmute(date = as.Date(date),
            tout = tout %>% tolower(),
            sport =sport %>% tolower(),
            bet_type=substr(sport, 5,nchar(sport)) %>% 
              stringr::str_replace_all('money line','moneyline') %>% 
              stringr::str_replace_all('sides','side'),
            result=`w/l` %>% tolower(),
            net) %>% 
  mutate(sport=substr(sport,1,3))
```
