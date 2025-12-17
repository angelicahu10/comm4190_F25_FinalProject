**Study Buddy Workflow**


Start: Student Interaction (A): The process begins with the student initiating the interaction.

STEP 1: Check Form Submission Status (B): The system first checks if the student has previously submitted the required profile form.

Form Submitted: YES (C): If the form was submitted, the DB Tool retrieves the existing Student Profile Data.Form Submitted: NO (D): If the form was not submitted, an Interactive LLM is used to gather all required data through a conversation.

Transition to Validation: Once data is gathered by the Interactive LLM (D), it proceeds to the next step (C/E).Data Validation LLM: Is Data Complete and Unambiguous? 

(E): The collected data is passed to a specialized LLM to check for quality and completeness

.Refinement Loop: No: Ambiguity Found (F): If ambiguity exists, a Clarification LLM asks one specific follow-up question.

Wait for Response / Integrate Data (G): The system waits for the student's response to the clarification and integrates the new data.

Finalized Student Profile Vector (I): Once the data is confirmed (E $\rightarrow$ I) or refined (G $\rightarrow$ I), a structured profile vector is created.

STEP 2: RUN COMPATIBILITY ANALYSIS (I): The finalized profile is ready for matching.

Run Matching Algorithm LLM: Scan DB for >6/10 Matches (J): The matching process begins by scanning the database for candidates who meet a preliminary compatibility score threshold.

Matching LLM: Calculate Compatibility Score (K): A dedicated LLM performs the detailed calculation of the final compatibility score between the student and the potential match.

Output Node: Present Match Profile & Reasoning (L): The result, including the match profile and the logic behind the score, is presented to the student.

Student Confirmation: Does Match Seem Ideal? (M): The system prompts the student for feedback on the quality of the presented match.

Outcome Routing: Yes: Ideal Match (N): If the student confirms the match is ideal, the system proceeds to confirm the match and summarize its strengths.

Outcome Routing: No: Not Ideal (O): If the student rejects the match, a Feedback LLM captures the user's reasoning for refinement.

STEP 3: WRAP UP (N): Once a match is confirmed (N).

Wrap Up: Encourage Contact & Offer Study Planning Assistance (P): The interaction is finalized with next steps and assistance offers.

End: Interaction Complete (Q): The session concludes successfully.

```mermaid
graph TD
    A[Start: Student Interaction] --> B{STEP 1: Check Form Submission Status};

    %% --- Conditional Path for Data Gathering ---
    B -->|Form Submitted: YES| C[DB Tool: Retrieve Student Profile Data];
    B -->|Form Submitted: NO| D(Interactive LLM: Gather All Required Data);
    D --> C;
    
    C --> E{Data Validation LLM: Is Data Complete and Unambiguous?};

    %% --- Data Refinement Loop (If Ambiguity Exists) ---
    E -->|No: Ambiguity Found| F(Clarification LLM: Ask 1 Follow-up Question);
    E -->|Yes: Data Confirmed| I;
    F --> G[Wait for Response / Integrate Data];
    G --> I[Finalized Student Profile Vector];

    %% --- STEP 2: RUN COMPATIBILITY ANALYSIS ---
    I --> J[Run Matching Algorithm LLM: Scan DB for >6/10 Matches];
    J --> K[Matching LLM: Calculate Compatibility Score];

    K --> L[Output Node: Present Match Profile & Reasoning];
    
    L --> M{Student Confirmation: Does Match Seem Ideal?};
    
    %% --- Outcome Routing ---
    M -->|Yes: Ideal Match| N[Confirm Match: Summarize Strengths];
    M -->|No: Not Ideal| O[Feedback LLM: Capture User Reasoning];
    O --> J; %% Re-run search or refine parameters based on feedback (Loop)
    
    %% --- STEP 3: WRAP UP ---
    N --> P[Wrap Up: Encourage Contact & Offer Study Planning Assistance];
    P --> Q[End: Interaction Complete];

    %% --- Styling ---
    style A fill:#D4E8D4, stroke:#4CAF50, stroke-width:2px;
    style Q fill:#D4E8D4, stroke:#4CAF50, stroke-width:2px;
    style B fill:#FFFACD, stroke:#FFD700;
    style E fill:#FFFACD, stroke:#FFD700;
    style D fill:#BBDEFB, stroke:#2196F3;
    style J fill:#BBDEFB, stroke:#2196F3;
    style K fill:#BBDEFB, stroke:#2196F3;
    style L fill:#E1BEE7, stroke:#9C27B0, stroke-width:2px;
    style M fill:#FFFACD, stroke:#FFD700;
```

**StreamSense Workflow (Content Reccommendation)**

Start: User Submits Movie Rating (A): The process begins when the user submits a movie rating (the primary interaction signal).

STEP 1: Check Rating Input Validity (B): The system first checks if the input is a valid rating or if it is a different type of input (e.g., a query, or adversarial text).

Input: Valid Rating (C): If the input is a valid rating, the DB Tool retrieves the User History and relevant Movie Attributes.

Input: Query or Adversarial (D): If the input is not a valid rating, a Guardrail LLM is used to reject the non-rating input.

Transition to Retrieval : After the Guardrail LLM handles the invalid input, the process conceptually moves to profile retrieval (C).

Data Validation LLM: Is Preference Profile Complete? (E): The retrieved data is passed to a specialized LLM to check if the user's overall preference profile is robust enough for accurate matching.

Refinement Loop: No: Ambiguity Found (F): If the profile is insufficient or ambiguous, a Clarification LLM asks one specific clarifying question.

Wait for Response / Integrate Clarification (G): The system waits for the user's response to the clarification and integrates the new data.

Finalized User Preference Vector (I): Once the data is confirmed (E $\rightarrow$ I) or refined (G $\rightarrow$ I), a structured profile vector representing user taste is created.

STEP 2: RUN COMPATIBILITY ANALYSIS (I): The finalized preference vector is ready for matching.

Run Matching Algorithm LLM: Scan DB for Top 4-7 Matches (J): The matching process begins by scanning the database for movies that meet a high compatibility threshold with the user's preference vector.

Matching LLM: Calculate Compatibility Score (K): A dedicated LLM calculates the final, detailed Compatibility Score between the user's taste and the potential movie recommendations.

Output Node: Present Recommendation Row & Reasoning (L): The result, including the movie recommendation(s) and the Reasoning behind the match, is presented to the user.

User Confirmation: Does Match Seem Ideal? (M): The system prompts the user for feedback on the quality of the recommendation(s).

Outcome Routing: Yes: Ideal Match (N): If the user confirms the match is ideal, the system confirms the match and summarizes the compatibility strengths.

Outcome Routing: No: Not Ideal (O): If the user rejects the match, a Feedback LLM captures the user's reasoning for refinement.

Re-run Search: The system uses the captured feedback to adjust search parameters and loops back to Run Matching Algorithm LLM (J) to generate better recommendations.

STEP 3: WRAP UP (N): Once a match is confirmed (N).Wrap Up: Encourage More Ratings to Fine-Tune (P): The interaction is finalized with a suggestion for the user to submit more ratings to continuously improve their preference profile.

End: Interaction Complete (Q): The session concludes successfully.

```mermaid
graph TD
    A[Start: User Submits Movie Rating] --> B{STEP 1: Check Rating Input Validity};

    %% --- Conditional Path for Data Gathering (Adapted for Content) ---
    B -->|Input: Valid Rating| C[DB Tool: Retrieve User History & Movie Attributes];
    B -->|Input: Query or Adversarial| D(Guardrail LLM: Reject Non-Rating Input);
    D --> C;
    
    C --> E{Data Validation LLM: Is Preference Profile Complete?};

    %% --- Data Refinement Loop (If Ambiguity Exists) ---
    E -->|No: Ambiguity Found| F(Clarification LLM: Ask 1 Clarifying Question);
    E -->|Yes: Data Confirmed| I;
    F --> G[Wait for Response / Integrate Clarification];
    G --> I[Finalized User Preference Vector];

    %% --- STEP 2: RUN COMPATIBILITY ANALYSIS (Adapted for Content) ---
    I --> J[Run Matching Algorithm LLM: Scan DB for Top 4-7 Matches];
    J --> K[Matching LLM: Calculate Compatibility Score];

    K --> L[Output Node: Present Recommendation Row & Reasoning];
    
    L --> M{User Confirmation: Does Match Seem Ideal?};
    
    %% --- Outcome Routing (Adapted for Content) ---
    M -->|Yes: Ideal Match| N[Confirm Match: Summarize Compatibility Strengths];
    M -->|No: Not Ideal| O[Feedback LLM: Capture User Feedback];
    O --> J; %% Re-run search or refine parameters based on feedback (Loop)
    
    %% --- STEP 3: WRAP UP ---
    N --> P[Wrap Up: Encourage More Ratings to Fine-Tune];
    P --> Q[End: Interaction Complete];

    %% --- Styling (Retained) ---
    style A fill:#D4E8D4, stroke:#4CAF50, stroke-width:2px;
    style Q fill:#D4E8D4, stroke:#4CAF50, stroke-width:2px;
    style B fill:#FFFACD, stroke:#FFD700;
    style E fill:#FFFACD, stroke:#FFD700;
    style D fill:#BBDEFB, stroke:#2196F3;
    style J fill:#BBDEFB, stroke:#2196F3;
    style K fill:#BBDEFB, stroke:#2196F3;
    style L fill:#E1BEE7, stroke:#9C27B0, stroke-width:2px;
    style M fill:#FFFACD, stroke:#FFD700;
```
