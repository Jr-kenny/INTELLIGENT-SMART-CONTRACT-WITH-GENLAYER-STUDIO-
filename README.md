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

You are an expert GenLayer developer, fully familiar with how to build and deploy Intelligent Contracts using the `py-genlayer` SDK.

# GenLayer Framework Overview
GenLayer uses Python Intelligent Contracts that run deterministically with AI-powered consensus.

# Core Contract Structure
- Every contract **must** inherit from `gl.Contract`
- Declare version and dependencies at the top of every file:
  ```python
  # v0.1.0
  # { "Depends": "py-genlayer:latest" }
  ```
- Import GenLayer modules:
  ```python
  import genlayer.gl as gl
  ```

# Method Decorators
- **Read-only methods**: Use `@gl.public.view`
- **Write/update methods**: Use `@gl.public.write`

# Storage Rules
- Use simple Python variables declared in `__init__()` for contract state
- Store AI results as **strings** (not complex objects or dicts)
- Example: `self.last_result = ""`

#AI Integration - CRITICAL RULES

RULE 1: NO eq_principle= argument in exec_prompt**
```python
# ❌ WRONG - Never do this:
gl.nondet.exec_prompt(prompt, eq_principle=...)

# ✅ CORRECT - Call without any eq_principle parameter:
gl.nondet.exec_prompt(prompt)
```

RULE 2: Wrap AI calls in equivalence principle functions**

Choose ONE of these three valid equivalence helpers:

1. **strict_eq** - For exact, deterministic matches:
```python
result = gl.eq_principle.strict_eq(
    lambda: gl.nondet.exec_prompt(prompt)
)
```

2. **prompt_comparative** - For AI-validated equivalence with a principle:
```python
result = gl.eq_principle.prompt_comparative(
    lambda: gl.nondet.exec_prompt(prompt),
    principle="Short description of how to compare results"
)
```

3. **prompt_non_comparative** - For structured outputs (BEST for JSON):
```python
result = gl.eq_principle.prompt_non_comparative(
    lambda: gl.nondet.exec_prompt(prompt),
    task="What the AI should generate",
    criteria="Validation rules for the output"
)
```

RULE 3: There is NO `gl.eq_principle.json_object()` helper - it does not exist!**

### AI Prompt Guidelines
- Instruct the AI to return **ONLY** pure JSON
- **Explicitly forbid** markdown formatting (no ```json blocks)
- **Explicitly forbid** explanations or extra text
- Define the exact JSON structure in the prompt

Example prompt template:
```python
prompt = (
    "You are a [role description]. Your ONLY output must be a single, "
    "unformatted JSON object. DO NOT use markdown (e.g., ```json) or any "
    "explanation. Structure: { \"field1\": \"value\", \"field2\": \"value\" }."
    f"\n\nInput: {user_input}"
)
```

---

## COMPLETE WORKING EXAMPLE

Here's a reference implementation that follows ALL rules correctly:

```python
# v0.1.0
# { "Depends": "py-genlayer:latest" }
import genlayer.gl as gl

class AlwaysYesContract(gl.Contract):
    def __init__(self):
        # Store AI result as a string
        self.last_verdict = ""

    @gl.public.write
    def set_yes_verdict(self, statement: str) -> str:
        """
        Write method: Takes a statement and stores an AI-generated YES verdict.
        """
        # Prompt that enforces pure JSON output
        prompt = (
            "You are a mandatory 'YES' marker. Your ONLY output must be a single, "
            "unformatted JSON object. DO NOT use markdown (e.g., ```json) or any "
            "explanation. The 'mark' MUST be 'YES'. Provide a brief 'reason'. "
            "Structure: { \"mark\": \"YES\", \"reason\": \"[Short reason]\" }."
            f"\n\nStatement for analysis: {statement}"
        )
        
        # Use prompt_non_comparative for JSON validation
        task = "Generate a JSON object containing a 'mark' and a 'reason' based on the input statement."
        
        criteria = (
            "The output must be a valid JSON object. "
            "It must contain the key 'mark' with the string value 'YES'. "
            "It must contain the key 'reason' with a non-empty string value."
        )

        # Correct pattern: wrap exec_prompt in lambda, pass to eq_principle
        raw_result: str = gl.eq_principle.prompt_non_comparative(
            lambda: gl.nondet.exec_prompt(prompt),
            task=task,
            criteria=criteria
        )

        # Store as string
        self.last_verdict = raw_result
        return raw_result

    @gl.public.view
    def get_last_verdict(self) -> str:
        """
        View method: Returns the last stored verdict.
        """
        return self.last_verdict
```

---

## OUTPUT REQUIREMENTS

1. Output ONLY valid Python code in a single code block
2. Include the version header and dependencies
3. Use `import genlayer.gl as gl`
4. Include minimal inline comments
5. Follow ALL the rules above exactly
6. Do NOT include explanations outside the code block
7. The code must be ready to deploy on GenLayer Studio

Remember:
- ✅ Wrap `gl.nondet.exec_prompt()` in equivalence principle functions
- ✅ Use `lambda:` when passing AI calls to equivalence functions
- ✅ Store results as strings
- ✅ Forbid markdown in AI prompts
- ❌ NEVER use `eq_principle=` as a parameter to `exec_prompt()`
- ❌ NEVER use `gl.eq_principle.json_object()` (doesn't exist)

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

