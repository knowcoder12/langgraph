# CRAG 
like if u have a scene where u give a document to a chatbot and tell it to answer from it
so it will try to find and give wrong answer as that info is not present but chabot is forced and have no choice other than that to give answer from 


so how crag solve this problem so crag do something different here so take a case u ask a question now retiever will fetch some document and like the fetch docs will not go to final llm it will first go to some llm which is present at mid will decide like :
1. the fetch docs is related to th question if yes it wil give doc toh next llm
2. if not it will use some extra tool web search etch to find answer
3. if its partially present so now it will do both find the answer from web and mix this with the fetch knowledge that is retreived


# video dekh le please 

# now this paper also suggest a way to improve the reasoning 

Like take a scenario where you have fetch some docs based on your query but as spliter split on baisis on no. of character there can be as situation wheere like two unrelated info is present in same chunk so if it is fetched for any query so it also have unrelated info in it so to handle we split the chunk more in sentence and send to a llm and based on that llm decision we decide if the info is needed or not 

but like video dekh as usme thoda alag yaha thoda alag atya maine 
