# lexRankr
### Extractive Text Summariztion with lexRankr (an R package implementing the LexRank algorithm)

lexRankr is an R implementation of the LexRank algorithm discussed by Güneş Erkan & Dragomir R. Radev in [LexRank: Graph-based Lexical Centrality as Salience in Text Summarization](http://www.cs.cmu.edu/afs/cs/project/jair/pub/volume22/erkan04a-html/erkan04a.html).  There are currently 7 functions in the package.  The main function is `lexRank()` with the remaining 6 functions serving as helpers to this function (or as a means for the user to build step by step to the output of `lexRank()`).

###Examples:
  ####lexRank
  ```
  library(lexRankr)
  library(magrittr)
  
   #for this example a single article from the acq dataset from the tm package
        #will be split into multiple documents to be passed to lexRank
  
  data("acq", package="tm")
  docs <- strsplit(acq[[4]]$content,"\n\\s{3,}") %>%
      .[[1]] %>%
      gsub(pattern="\n", replacement = " ")
  
  #note1: the lexrank algorithm is designed to work on a cluster of documents.
          #LexRank is built on the idea that a cluster of docs will focus on similar topics.
      
  #note2: pairwise sentence similiarity is calculated for the entire set of documents passed to the function.  
          #This can be a computationally instensive process (esp with a large set of documents)
  
  (topSentsDf <- lexRank(docs))
  ```
  ####Using helper functions for lexRank
  ```
  library(lexRankr)
  library(magrittr)
  
   #for this example a single article from the acq dataset from the tm package 
        #will be split into multiple documents to be passed to lexRank
  
  data("acq", package="tm")
  docs <- strsplit(acq[[4]]$content,"\n\\s{3,}") %>%
      .[[1]] %>%
      gsub(pattern="\n", replacement = " ")
      
  sentenceTokenList <- sentenceTokenParse(docs)
  sentenceDf <- sentenceTokenList$sentences
  tokenDf <- sentenceTokenList$tokens
  similDf <- sentenceSimil(tokenDf$sentenceId, tokenDf$token)
  topSentIdsDf <- lexRankFromSimil(similDf$sent1, similDf$sent2, similDf$similVal)
  
  (topSentsDf <- dplyr::inner_join(sentenceDf, topSentIdsDf, by=c("sentenceId"="sentenceId"))
  
  ```
