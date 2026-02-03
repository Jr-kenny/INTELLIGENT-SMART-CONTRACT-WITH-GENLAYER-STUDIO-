# INTELLIGENT-CONTRACT-WITH-GENLAYER-STUDIO-
EASY FUN STEPS TO DEPLOYING INTELLIGENT SMART CONTRACT WITH GENLAYER STUDIO 
# Deploying an Intelligent Contract on GenLayer Studio

A step-by-step beginner’s guide to creating and deploying your first **Intelligent Contract** using **GenLayer Studio** and an **AI /LLM (Language Model)** such as ChatGPT, Claude, or Gemini.


## Overview
Before we begin what is the easiest way to describe genlayer?

**GenLayer** is a blockchain development platform that allows you to build **AI-native smart contracts** in Python.

in my own way GenLayer is like giving smart contracts a brain.
Normally, smart contracts can only follow fixed rules like “if this, then that, disburse only this, block that etc ”
They can’t think, reason, or adapt.

But GenLayer changes that.
It connects blockchain logic to large language models (LLMs)  like the same kind of AI that powers ChatGPT  directly inside the contract executing flow .

That means you can write contracts that don’t just store or transfer data only, They can analyze, decide, and generate responses on-chain.

## What Is an Intelligent Contract?

An Intelligent Contract is a contract that can think before acting.

It still lives onchain like any normal contract, but it can talk to an AI model (through GenLayer) to make decisions or generate outputs

In short:

Traditional smart contract = logic-only

Intelligent contract = logic + AI reasoning


Unlike traditional smart contracts that only follow pre-coded rules, *Intelligent Contracts* can reason, make context-aware decisions, and adapt their behavior.

This guide will walk you through:
1. Setting up your test environment  
2. Using an LLM to generate an intelligent contract  
3. Deploying it directly on **GenLayer Studio**

---

## 1. Claim Faucet Tokens

To deploy and test your contract, you need test tokens.

- Visit the official GenLayer Testnet Faucet:  
  [https://genlayer-testnet.hub.caldera.xyz/](https://genlayer-testnet.hub.caldera.xyz/)
- Claim your faucet tokens.  
- These tokens will cover deployment and gas fees.


## 2. Explore GenLayer Studio

Access the contract editor and environment:

- Go to [https://studio.genlayer.com/contracts](https://studio.genlayer.com/contracts)

- ## Review existing sample contracts
- <img width="959" height="472" alt="Screenshot 2025-11-06 073132" src="https://github.com/user-attachments/assets/8e48b293-a4e2-4df5-8d2d-9d063a75bf26" />

- ## Experiment with deploying a few to understand the interface and transaction flow
<img width="959" height="440" alt="Screenshot 2025-11-06 073405" src="https://github.com/user-attachments/assets/156dab37-ab04-4d66-8167-a1e443064377" />


## 3. knowing some fun way you can make use of intelligent contracts  

Example use cases:
- Autonomous moderation systems  
- Smart voting or DAO agents  
- Adaptive reward contracts  
- AI-powered or judges  

## 4. Generate a Contract Using an LLM

We’ll now use an Ai to generate your contract code.  
Copy the prompt below and paste it into ChatGPT, Claude, or Gemini.

EDIT THE PART I SAID YOU SHOULD EDIT 
---

### **Prompt for Your Ai **

````
(START HERE - REPLACE THE TEXT BELOW WITH YOUR CONTRACT IDEA AND THEN DELETE THIS WITH ‼️)

YOUR CONTRACT IDEA:
[Delete this line and the example and describe what you want the contract to do]

Example: "Create a content moderation contract that analyzes user-submitted text and determines if it's safe or unsafe. The AI should return JSON with: {"status": "safe|unsafe", "category": "hate_speech|spam|violence|safe", "confidence": "high|medium|low", "reason": "brief explanation"}. Store all moderation decisions."


PLEAS STOP - Don't edit anything below this line  ‼️
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

---

You are an expert GenLayer developer, specialized in building Intelligent Contracts using the py-genlayer SDK. Your task is to generate a complete, deployable Python contract based on the user's specific requirements while strictly adhering to the architectural and security rules of the GenLayer protocol.
1. CORE CONTRACT STRUCTURE
• Version Header: Every file must start with: # { "Depends": "py-genlayer:test" }.
• Imports: Use from genlayer import * to ensure all sized types and modules are in the global scope.
• Class Declaration: Every contract must inherit from gl.Contract.
• Public Methods:
    ◦ Use @gl.public.view for read-only state access.
    ◦ Use @gl.public.write for state modifications.
    ◦ Use @gl.public.write.payable for methods that receive GEN tokens via gl.message.value.
2. PERSISTENT STORAGE RULES
• Class-Level Declaration: All persistent fields must be declared in the class body with type annotations. Fields declared only inside __init__ via self.field = x are not persistent and will be discarded.
• Mandatory Collection Types:
    ◦ Replace list[T] with DynArray[T].
    ◦ Replace dict[K, V] with TreeMap[K, V].
• Sized Integers: Do not use the standard Python int for storage. Use sized types like u256, i32, or u64. Use bigint only if arbitrary precision is required.
• Addresses: Use the Address type for all account-related logic. Normalize inputs using Address(hex_string).
3. AI INTEGRATION & NON-DETERMINISM
• Wrapper Requirement: All AI calls (gl.nondet.exec_prompt) must be wrapped in an equivalence principle function.
• Valid Principles:
    1. gl.eq_principle.strict_eq(lambda: ...) – For exact matches (e.g., booleans, exact web data).
    2. gl.eq_principle.prompt_comparative(lambda: ..., principle=...) – For results that can vary within a margin.
    3. gl.eq_principle.prompt_non_comparative(lambda: ..., task=..., criteria=...) – Best for JSON/NLP. Validators evaluate the leader's output against criteria.
• Lambda Usage: Always pass the non-deterministic logic as a lambda to the equivalence helper.
• Constraint: gl.nondet.exec_prompt must never have an eq_principle argument.
• Storage Isolation: Storage is inaccessible from non-deterministic blocks. If you need storage data inside an AI block, you must use gl.storage.copy_to_memory(self.variable) before the block begins.
4. AI PROMPT & OUTPUT GUIDELINES
• Prompt Content: Instruct the AI to return ONLY pure JSON. Explicitly forbid markdown (no ```json blocks), explanations, or conversational text.
• Error Handling: Use raise gl.Rollback("error_code") to trigger a state revert if AI consensus fails or logic requirements are not met.
5. REFRACTORING EXAMPLE
If the logic involves getting a web page and analyzing it:
# { "Depends": "py-genlayer:test" }
from genlayer import *

class ExampleContract(gl.Contract):
    last_result: str # Declared at class level for persistence

    def __init__(self):
        self.last_result = ""

    @gl.public.write
    def analyze_web(self, url: str):
        # Step 1: Copy needed state to memory (if any)
        # Step 2: Define non-det logic
        def nd_logic():
            page = gl.get_webpage(url, mode="text")
            prompt = f"Analyze this: {page}. Return JSON only."
            return gl.nondet.exec_prompt(prompt)
        
        # Step 3: Wrap in principle
        self.last_result = gl.eq_principle.prompt_non_comparative(
            nd_logic,
            task="Analyze webpage",
            criteria="Must be valid JSON"
        )
OUTPUT REQUIREMENTS
1. Output ONLY valid Python code in a single code block.
2. Include the required version header and from genlayer import *.
3. Ensure all state variables are annotated in the class body.
4. Do not include any explanations outside the code block.
Generate the contract now based on the user's requirements at the top of this prompt.
````


## 5. Deploy Your Contract

Once your LLM sens you the contract code:

Open [GenLayer Studio]([https://studio.genlayer.com/])

## Click New Contract
<img width="959" height="474" alt="Screenshot 2025-11-06 073945" src="https://github.com/user-attachments/assets/048c88c9-ab70-4217-8316-073585eb7579" />


## Name it (e.g., myfirstcontract.py)
<img width="959" height="440" alt="Screenshot 2025-11-06 074950" src="https://github.com/user-attachments/assets/b9a9bc52-56fe-4dd0-81b4-5d0f2eb19ffb" />


Paste the code into the editor

## Click Deploy & Debug → Deploy
<img width="959" height="438" alt="Screenshot 2025-11-06 075504" src="https://github.com/user-attachments/assets/609158a3-a9a5-43fa-84d4-cca7710eac98" />


## Approve the transaction 
<img width="1919" height="948" alt="image" src="https://github.com/user-attachments/assets/e3f7a539-327e-436d-b937-49cb43be516d" />


## You’ve successfully deployed your first Intelligent Smart Contract.

<img width="443" height="873" alt="image" src="https://github.com/user-attachments/assets/459d76d5-8379-4950-8bd6-69e5956e7724" />

## 6. Playground 

After deployment, you can:

Interact with your contract directly in Studio

Modify prompts or logic and redeploy

Combine multiple contracts to create intelligent systems, ( just edit the first prompt to your liking)

just keep experimenting there lots of fun in the studio 




GenLayer discord : https://discord.gg/BSAfQhBxdz

Official GenLayer Documentation : https://docs.genlayer.com/


https://x.com/Jrken_ny

