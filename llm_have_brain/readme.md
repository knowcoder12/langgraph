LLM dont have memory 
first thing chagpt and some gemni model have hight context length means you can send 
many things words roughly 128k to 1million size of data inside a llm

2. llm also develop capablity that it can use extra data that is sende with it to generate
3. answer that is not know to the model

4. In llm like model dont have memory so whenever you send new chat with new chat you also send old messages too like
5. above msg also get concatinate and sended with them

6. and like in chagpt one chat have its own memory this is called short term memory
7. evry chat have its own memory that cant be access in other chat and this short tem memory is called one thread

Now main problem as we continuing chat the chat will become big so to handle we first extract last three conversation and abve all converstion we summarised by ai 
so now summarization+last 3 chat will be given as input o by this we can ensure that context window problem can be handled in a good way 

## long term memory 
somethings like ur name or details should be saved for longer period of time that can be useful in other chats too
it have to slective while choosing what elemnt to add in this 

three types of ltm or three diff type of things u store in long term memory 
episodic memory : past events and experience it store like user have reject going to an event last time , user reject any solution 
semantic memory : facts knowledge and stable info stored example user love java user study in this college 

process memory : strategies rules that user like to follow during converation like user like details soln or soln in story format etc 



how ltm formed
1. candidates chosen : first you check the chta rmove the noise and check if there any elevent details that can be added to ltm
2. storgae : after filtering and geting best cnadidate for being part of ltm u add it into ltm
3. retirval : during conversation u dont get full ltm loaded into the chat u retrive do search if any thing is required from ltm for answering this qiestion
4. injection : then if yes u inject or send the detail with the question or context input 
