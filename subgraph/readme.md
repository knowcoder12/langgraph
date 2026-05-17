subgraph idea means that nodes in a graph itself is a graph 
we can make multi agents using this every node is a self agent hich do something 


benfit:
reuseablity
modularity -: means codes if in diffrent func 
and if one subgraph fails whole graph doesnt get fail 

types of subgraph:
these are of two types understand it -> watch the video
1. you ust add a trigger function inside a node so when that node get rigger it trigger some other graph too-> in this we have state for both these graph seprate
2. in this type u directly add whole subgraph as a node in this the subgrah state is same as of parents 


# how to add persistance in langgraph : chagpt se kar 
example code :

        from typing import TypedDict, Annotated
        
        from langgraph.graph import (
            StateGraph,
            START,
            END
        )
        
        from langgraph.graph.message import add_messages
        
        from langgraph.checkpoint.memory import MemorySaver
        
        from langchain_core.messages import (
            HumanMessage,
            AIMessage
        )
        
        from langchain_openai import ChatOpenAI
        
        
        # ==========================================
        # STATE
        # ==========================================
        
        class State(TypedDict):
            messages: Annotated[list, add_messages]
        
        
        # ==========================================
        # LLM
        # ==========================================
        
        llm = ChatOpenAI(
            model="gpt-4o-mini",
            temperature=0
        )
        
        
        # ==========================================
        # SUBGRAPH
        # RESEARCH AGENT
        # ==========================================
        
        research_builder = StateGraph(State)
        
        
        def research_node(state: State):
        
            response = llm.invoke([
                {
                    "role": "system",
                    "content": """
                    You are a research expert.
        
                    Remember previous research
                    from earlier conversations.
                    """
                }
            ] + state["messages"])
        
            return {
                "messages": [response]
            }
        
        
        research_builder.add_node(
            "research_node",
            research_node
        )
        
        research_builder.add_edge(
            START,
            "research_node"
        )
        
        research_builder.add_edge(
            "research_node",
            END
        )
        
        
        # ==========================================
        # PERSISTENT SUBGRAPH
        # ==========================================
        
        research_graph = research_builder.compile(
            checkpointer=True
        )
        
        # IMPORTANT:
        # checkpointer=True
        # means:
        # this subgraph remembers previous chats
        
        
        # ==========================================
        # MAIN GRAPH
        # ==========================================
        
        main_builder = StateGraph(State)
        
        
        def main_chat(state: State):
        
            response = llm.invoke(state["messages"])
        
            return {
                "messages": [response]
            }
        
        
        # CALL SUBGRAPH
        def call_research_subgraph(state: State):
        
            result = research_graph.invoke(
                state,
                config={
                    "configurable": {
                        "thread_id": "research-thread-1"
                    }
                }
            )
        
            return {
                "messages": result["messages"]
            }
        
        
        main_builder.add_node(
            "main_chat",
            main_chat
        )
        
        main_builder.add_node(
            "research_agent",
            call_research_subgraph
        )
        
        
        # FLOW
        main_builder.add_edge(
            START,
            "research_agent"
        )
        
        main_builder.add_edge(
            "research_agent",
            END
        )
        
        
        # ==========================================
        # MAIN GRAPH CHECKPOINTER
        # ==========================================
        
        memory = MemorySaver()
        
        app = main_builder.compile(
            checkpointer=memory
        )
        
        
        # ==========================================
        # FIRST CALL
        # ==========================================
        
        config = {
            "configurable": {
                "thread_id": "main-thread"
            }
        }
        
        result1 = app.invoke(
            {
                "messages": [
                    HumanMessage(
                        content="Research Apple company"
                    )
                ]
            },
            config=config
        )
        
        print("\nFIRST CALL\n")
        
        for m in result1["messages"]:
            print(m.content)
        
        
        # ==========================================
        # SECOND CALL
        # ==========================================
        
        result2 = app.invoke(
            {
                "messages": [
                    HumanMessage(
                        content="Now compare it with Microsoft"
                    )
                ]
            },
            config=config
        )
        
        print("\nSECOND CALL\n")
        
        for m in result2["messages"]:
            print(m.content)
