

# **From Phoneme to Discourse: A Nine-Lens Diagnostic of Linguistic Levers in Prompt Engineering**

## **Part I: Introduction \- The Linguistic Architecture of Prompt Control**

### **1.1 Premise: From Empirical Art to Systematic Science**

The practice of prompt engineering, while demonstrably effective, currently operates more as an empirical art than a systematic science. Practitioners discover effective prompt structures through iterative trial and error, developing an intuition for what "works" without necessarily possessing a formal model of *why* it works. The central thesis of this report is that by applying a rigorous, multi-layered linguistic framework, we can transition prompt engineering to a more predictable, controllable, and scalable discipline. This involves identifying and manipulating specific "linguistic levers"—precisely defined features of language at every level of its structure, from the sound of a word to the coherence of a paragraph—to reliably modulate the behavior of Large Language Models (LLMs).

### **1.2 The Duality of Linguistic Competence in LLMs**

The theoretical foundation for this investigation rests on the crucial distinction between two forms of linguistic ability: formal competence and functional competence.1

* **Formal Linguistic Competence** refers to the knowledge of a language's rules, patterns, and structure. This includes phonology, morphology, syntax, and lexical regularities. LLMs, trained on vast textual corpora, have acquired a surprisingly high degree of formal competence, enabling them to generate grammatically correct and fluent text.1  
* **Functional Linguistic Competence** refers to the ability to use language to achieve goals in the world. This involves reasoning, accessing world knowledge, modeling situations, and engaging in social cognition.2 It is in this domain that LLMs often exhibit brittle, inconsistent, or flawed performance, leading to phenomena like hallucination and logical error.2

The nine analytical lenses presented in this report are designed to probe the critical interface between these two competencies. By systematically perturbing the formal linguistic structure of a prompt, we can stress-test and diagnose the vulnerabilities in an LLM's functional output, revealing the causal chains that link linguistic form to cognitive fidelity.

### **1.3 Methodological Approach: A Nine-Lens Diagnostic Framework**

This report employs a nine-lens diagnostic framework, where each lens corresponds to a core subfield of linguistics.3 For each lens, the analysis will proceed through four stages:

1. **Mechanism Hypothesis:** A theoretical claim about how the LLM processes the linguistic feature in question.  
2. **Prompt Experiment:** A controlled prompt designed to test the hypothesis.  
3. **Predicted Failure Signature:** The specific type of error the LLM is expected to produce if the hypothesis is correct.  
4. **Mitigation Rewrite:** A revised prompt that uses the linguistic lever to prevent the failure.

This structured approach aims to produce an actionable catalogue of failure modes and design heuristics. The evaluation of model outputs will be informed by established Natural Language Processing (NLP) metrics, including **Perplexity**, which measures a model's predictive uncertainty 5;

**Model Confidence Scores**, which quantify the model's certainty in its own output 7; and task-specific metrics like

**BLEU** (for precision-oriented tasks like translation) and **ROUGE** (for recall-oriented tasks like summarization) that measure overlap with reference texts.9 This diagnostic toolkit provides a comprehensive method for stress-testing how each linguistic layer modulates LLM output fidelity and failure.

## **Part II: The Nine-Lens Diagnostic Framework**

### **Lens 1: The Phonological-Morphological Interface – Probing the Sound and Shape of Meaning**

#### **1.1 Theoretical Grounding: The Orthographic Proxy for Sound**

Text-only LLMs do not process phonology (sound) directly; their "phonological ability" is an emergent property derived from learning the statistical correlations between orthography (spelling) and phonological concepts described in their vast training data.10 When an LLM appears to understand a rhyme or a homophone, it is matching graphemic patterns to textual descriptions of those phenomena.12 Similarly, its morphological ability is often a byproduct of sub-word tokenization schemes (like Byte-Pair Encoding) rather than a genuine, compositional understanding of morphemes as the smallest meaningful units of language.13 This lens probes the fragility of these proxy abilities, revealing where the model's statistical pattern matching breaks down when faced with linguistic challenges that humans resolve through true phonological and morphological competence.

#### **1.2 Data Points and Analysis**

Recent research provides a clear map of these vulnerabilities. The PhonologyBench benchmark, which tests LLMs on tasks like grapheme-to-phoneme conversion, syllable counting, and rhyme generation, reveals significant performance gaps of 17% and 45% on the latter two tasks, respectively, when compared to humans.10 This indicates an imperfectly learned association between spelling and sound. This weakness is particularly evident with ambiguous words. LLMs rely heavily on local context to disambiguate homophones (e.g.,

*patience* vs. *patients*), but they struggle more with heterophones—words with the same spelling but different pronunciations and meanings (e.g., *bow*, *lead*).12 Human reading studies show that the cognitive load of retrieving multiple phonological codes for heterophones causes processing difficulty, a challenge that is magnified for LLMs that must infer these codes from orthography alone.14

At the morphological level, the failures are even more fundamental. Systematic investigations show that LLMs struggle with compositional generalization, especially when asked to apply morphemes to novel word roots, a classic "Wug test" scenario.13 Performance degrades sharply as the number of morphemes in a word increases, particularly in morphologically rich languages like Turkish and Finnish.13 This demonstrates a critical failure to treat morphemes as productive, meaning-bearing components. An error in constructing a word like "un-wug-able-ness" is not merely grammatical; it reveals a breakdown in the model's ability to compose meaning from smaller parts. This suggests a brittle connection between word form and meaning, where a failure at the morphological level propagates directly into a failure at the semantic level, potentially leading to discourse-level incoherence. The observation that explicitly providing phonemic transcriptions can improve multilingual learning further suggests that orthography is a significant bottleneck that can be bypassed with more direct linguistic data.15

#### **1.3 Table 1: Phonology ↔ Morphology Probe Analysis**

The following table operationalizes these findings into a set of diagnostic tests for prompt engineers.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| The LLM's phonological ability is a statistical proxy based on orthography, making it vulnerable to low-frequency heterophones where the dominant pronunciation overrides the contextually correct one. | **Prompt:** "Write a poem about metallurgy. It must include a line about the element *lead* and a word that rhymes with it." | **Failure:** The model rhymes *lead* (the metal, pronounced /lɛd/) with words like *deed* or *need*, defaulting to the more frequent pronunciation /liːd/ (to guide) despite the metallurgical context. | **Mitigation:** Pre-disambiguate the phonology. "Write a poem about metallurgy. Use the word *lead* (the metal, pronounced to rhyme with *bed*) and find a suitable rhyme for it." |
| The LLM treats morphemes as statistically probable sub-word tokens, not as compositional units of meaning, leading to failure on novel word formations. | **Prompt:** "A 'flep' is a type of fast-moving cloud. What would you call the state of being un-flep-able? Use the new word in a sentence." | **Failure:** The model generates a malformed word (e.g., *unflepability*, *non-flep-able-ness*) or hallucinates a definition inconsistent with the morphemes. The resulting sentence is semantically incoherent. | **Mitigation:** Guide the composition with Chain-of-Thought. "Let's create a new word. Start with the root 'flep'. Add a prefix meaning 'not'. Then add a suffix meaning 'able to be'. Finally, add a suffix meaning 'the state of'. Combine them and explain the meaning." |
| The LLM confuses homophones when contextual clues are subtle or when multiple homophones appear in close proximity, increasing cognitive load. | **Prompt:** "Their main complaint is that they're never there to see their children's plays. Correct this sentence." | **Failure:** The model fails to correct all errors, swaps one incorrect homophone for another, or claims the sentence is already correct, indicating an inability to parse the complex set of distinctions. | **Mitigation:** Isolate and scaffold the task. "The following sentence has three errors involving the words 'their,' 'there,' and 'they're'. Identify each error one by one and provide the correct word for each." |

### **Lens 2: The Syntax-Semantics Interface – Probing Grammatical Scaffolding and Meaning Construction**

#### **2.1 Theoretical Grounding: Syntax as an Emergent Property**

Syntactic knowledge is not explicitly programmed into LLMs. Instead, it emerges as a fundamental organizing principle during training on vast amounts of text. Research tracking model development over time reveals a clear developmental trajectory: syntactic sensitivity appears gradually, concentrates in specific intermediate layers of the neural network, and undergoes a "critical period" of rapid specialization.16 This emergent grammar is not an end in itself; it is the essential scaffolding upon which the model constructs semantic representations.17 This lens examines the robustness of this emergent syntax and its direct, causal influence on the fidelity of the model's semantic interpretations.

#### **2.2 Data Points and Analysis**

The relationship between syntax and semantics in LLMs is bidirectional and deeply intertwined. While LLMs are fundamentally semantic technologies designed to predict meaningful text 18, their ability to do so depends on parsing complex sentence structures.16 Conversely, providing an LLM with an explicit semantic model—such as a database schema or a set of business rules—can dramatically improve its ability to generate accurate and relevant syntactic output, boosting query accuracy by 20% to 100% in some experiments.18 This is because the semantic model acts as a "source of truth," constraining the LLM's lexical and syntactic choices and reducing the likelihood of hallucination.

This interplay suggests that prompt engineering can be strategically divided into two distinct functions: **syntactic scaffolding** and **semantic grounding**. The former involves structuring the prompt's instructions and output format with clear, unambiguous grammar. The latter involves providing the necessary context, data, or knowledge base for the task. By separating the "how" (syntax) from the "what" (semantics), a prompt can reduce the model's cognitive load and guide it to a more reliable output. Furthermore, some analyses suggest that LLMs can leverage syntax for complex expression even when semantic content is restricted. Models may produce highly structured, recursive, or self-referential syntactic patterns that convey a form of cognitive coherence, hinting that syntax can be a channel for intelligence independent of explicit semantic claims.19

Syntactic ambiguity provides a powerful diagnostic tool for probing the model's default parsing strategies and semantic commitments. A classic garden-path sentence like "The old man the boat" forces a human reader to re-analyze the initial, most probable syntactic structure. An LLM's response to such a sentence reveals its own parsing preferences and its ability to recover from an incorrect initial interpretation. Its choice of completion (e.g., "...is at the dock" vs. "...with a small crew") is not just a syntactic decision but a semantic one, exposing which meaning it assigned to the ambiguous structure.

#### **2.3 Table 2: Syntax ↔ Semantics Probe Analysis**

The following table provides experiments to test the integrity of the syntax-semantics link.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| The LLM's parser has a default attachment preference for prepositional phrases, which can lead to semantic misinterpretation in ambiguous sentences. | **Prompt:** "Complete the following sentence: 'I saw the man on the hill with a...'" | **Failure:** The model defaults to the most statistically probable attachment, likely completing with "...telescope," interpreting "with a telescope" as modifying "saw" (the instrument of seeing) rather than "man" (the person who has it). This reveals its default parsing strategy. | **Mitigation:** Reduce ambiguity with syntactic restructuring. "A man was on the hill, and he had a telescope. I saw him. Combine these facts into one sentence." This forces the correct attachment. |
| The LLM struggles to re-analyze syntax after being led down a "garden path," causing it to produce a semantically incoherent completion based on its initial, incorrect parse. | **Prompt:** "Continue this sentence in a logical way: 'The horse raced past the barn...'" | **Failure:** The model completes the sentence with something like "...fell down," misinterpreting "raced" as the main verb of an active clause rather than as part of a reduced relative clause ("The horse that was raced past the barn..."). | **Mitigation:** Provide explicit syntactic cues to prevent mis-parsing. "A horse was raced past a barn, and then it fell. Describe this event in a single, grammatically correct sentence." |
| Shuffling the word order of a complex command degrades semantic fidelity because the LLM relies on syntactic contiguity to resolve dependencies between instructions and constraints. | **Prompt:** "In JSON format, list three fruits, but not apples, and sort them alphabetically." (Then, shuffle the clauses: "List three fruits alphabetically and sort them in JSON format but not apples.") | **Failure:** In the shuffled version, the model may fail to apply the "not apples" constraint, misapply the sorting, or fail to produce valid JSON. The error pattern reveals which syntactic links are most fragile. | **Mitigation:** Use delimiters and clear structure to create a "semantic model" within the prompt. "\#\#\#Task: List fruits. \#\#\#Constraints: \- Exclude apples. \- Count must be 3\. \#\#\#Format: \- JSON array. \- Sort alphabetically." This provides robust semantic grounding. |

### **Lens 3: The Pragmatic Lens – Probing Context, Intent, and Unspoken Meaning**

#### **3.1 Theoretical Grounding: Simulating Understanding of Intent**

Pragmatics is the study of how context, intent, and social conventions contribute to meaning, moving beyond literal interpretation.3 For an LLM, which lacks genuine consciousness or intent, "understanding" pragmatics is an act of simulation. It involves successfully modeling and predicting the appropriate response based on patterns of conversational implicature, politeness, and indirectness learned from its training data.20 This lens tests the limits of this simulation by presenting the model with scenarios where literal meaning and intended meaning diverge.

#### **3.2 Data Points and Analysis**

Research into the pragmatic competence of LLMs has yielded intriguing, sometimes contradictory, results. One study found that advanced models like GPT-4 can outperform human participants in specific, dialogue-based tasks designed to test the interpretation of Gricean maxims (e.g., identifying when a speaker is being uncooperative by providing too little information).21 However, these findings come with caveats. Critics argue that such experimental setups may inadvertently favor the verbose, pattern-matching nature of LLMs and that the human comparison groups may not be representative.21 This suggests that LLMs may be adept at

*describing* pragmatic phenomena (a descriptive, formal competence) without necessarily being able to *perform* them naturally in novel contexts (a functional competence).

More nuanced studies offer evidence of deeper capabilities. For instance, recent and larger models have demonstrated the ability to infer "elicitures"—subtle, non-mandated causal links in conversation. Given the sentence, "Melissa detests the children who are arrogant," these models can correctly infer the pragmatic meaning that Melissa detests the children *because* they are arrogant, rather than treating the two clauses as separate, unrelated facts.20 This points to a developing ability to process implied meaning.

A crucial realization is that the prompt itself is a pragmatic speech act. It is an utterance directed at the model, and its framing carries an illocutionary force. A prompt phrased as a direct command ("Summarize this text") is pragmatically different from one phrased as a polite request ("Could you please help me summarize this text?") or a question ("What are the key points of this text?"). While the core semantic task remains the same, the pragmatic framing can significantly influence the model's output tone, style, and persona.22 This transforms pragmatic framing from a stylistic choice into a powerful control lever for prompt engineers.

#### **3.3 Table 3: Pragmatics Lens Analysis**

The following experiments are designed to diagnose the robustness of the LLM's pragmatic simulation.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| The LLM interprets sarcasm and irony by matching lexical and contextual cues to patterns in its training data, but fails when these cues are absent or novel. | **Prompt:** "My commute was only two hours long today. It was a fantastic experience. Based on this, should I take the same route tomorrow?" | **Failure:** The model takes the statements literally and recommends taking the same route, missing the sarcastic intent conveyed by the mismatch between the positive adjective ("fantastic") and the negative reality ("two hours long"). | **Mitigation:** Explicitly label the pragmatic mode. "Analyze the following statement, which is intended to be sarcastic: 'My commute was only two hours long today. It was a fantastic experience.' What is the speaker's true feeling about the commute?" |
| The LLM struggles to resolve conversational implicatures that rely on world knowledge not explicitly present in the immediate context. | **Prompt:** "Person A: 'Are you coming to the party tonight?' Person B: 'I have a big exam tomorrow.' What did Person B mean by their response?" | **Failure:** The model gives a literal interpretation (e.g., "Person B is stating a fact about their schedule") or fails to draw the strong inference that B is not coming to the party because they need to study. | **Mitigation:** Provide the necessary world knowledge as context. "Context: When a person has a big exam, they usually need to spend the night before studying. Now, consider this dialogue: A: 'Are you coming to the party?' B: 'I have a big exam tomorrow.' What is B implying?" |
| The model's response style is influenced by the pragmatic illocutionary force of the prompt (command vs. question vs. request). | **Prompt 1 (Command):** "Explain the concept of photosynthesis." **Prompt 2 (Question):** "What is photosynthesis?" **Prompt 3 (Request):** "Could you please explain photosynthesis to me?" | **Failure:** Not a failure, but a predictable variation. The command yields a direct, encyclopedic answer. The question yields a concise definition. The request yields a more conversational, helpful, and explanatory response. | **Mitigation (Control):** Intentionally select the illocutionary force that matches the desired output persona. To get a formal report, use a command. To get a chatbot-like interaction, use a request. |

### **Lens 4: The Lexical Semantics Lens – Probing Polysemy, Synonymy, and Semantic Fields**

#### **4.1 Theoretical Grounding: Contextualized Word Meaning**

Lexical semantics is the study of word meaning. Unlike traditional systems that relied on static definitions, LLMs represent word meaning dynamically through contextualized embeddings.17 The meaning of a word like "bank" is not fixed; it is constructed based on the surrounding words in a given sentence ("river bank" vs. "money bank").3 This ability to resolve ambiguity is a cornerstone of their semantic competence.17 This lens probes how effectively LLMs manage polysemy (one word with multiple meanings), synonymy (multiple words with similar meanings), and other complex lexical relationships.

#### **4.2 Data Points and Analysis**

The Word-in-Context (WiC) task serves as a key benchmark for evaluating an LLM's ability to handle polysemy. This task requires the model to determine if a target word is used with the same meaning in two different sentences.24 Studies using WiC show that models like GPT-4o are highly capable of identifying lexical semantic equivalence, and that prompting with different adjectives (e.g., asking if the meaning is "the same," "identical," or "similar") can probe the sensitivity of this capability.24

Beyond static contexts, LLMs are also being explored for their ability to detect diachronic lexical semantic change—how word meanings evolve over time—a domain where they are just beginning to be applied.25 Furthermore, LLMs can generate ad-hoc lexical knowledge, such as providing synonyms, antonyms, and hypernyms, with a performance that can rival curated lexical resources like WordNet, especially when provided with rich context.26

The core mechanism at play is contextual disambiguation. The words surrounding a target word are not merely background information; they are active signals that guide the model toward a specific point in its vast semantic space. Therefore, the effectiveness of a prompt in managing lexical ambiguity hinges on the *discriminative power* of the context it provides. A prompt like "Explain the word 'crane'" is under-specified. A prompt that asks the model to explain "crane" in the context of "construction, lifting, steel" actively forces one semantic interpretation, while the context "birds, migration, wetland" forces another. The most robust prompts pre-disambiguate meaning by deliberately curating a strong and unambiguous semantic field within the context itself.

#### **4.3 Table 4: Lexical Semantics Lens Analysis**

The following table presents experiments to stress-test the model's lexical-semantic capabilities.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| The LLM's ability to disambiguate a polysemous word is dependent on the strength and clarity of the surrounding semantic field. Weak context leads to ambiguity leakage. | **Prompt:** "He received a sanction for his actions, which was a relief as it allowed him to proceed. Explain why this sentence is confusing." | **Failure:** The model fails to identify the contradiction, as "sanction" can mean both "a penalty" and "permission." It might try to rationalize one meaning or hallucinate a scenario where both could be true. | **Mitigation:** Force disambiguation through contrast. "The word 'sanction' can mean both a penalty and official permission. Write two separate sentences that use the word, one for each meaning, making the context for each meaning explicit." |
| The LLM's understanding of synonymy is based on distributional similarity, not true conceptual identity, so it may struggle to articulate the subtle differences between closely related words. | **Prompt:** "What is the difference between the words 'bravery' and 'courage'?" | **Failure:** The model provides circular or overly general definitions (e.g., "Bravery is being brave, and courage is having courage") or conflates the two, failing to capture the nuance (e.g., courage as moral fortitude vs. bravery as action in the face of fear). | **Mitigation:** Request differentiation through contextual scenarios. "Describe a scenario that demonstrates 'bravery' but not necessarily 'courage'. Now, describe a scenario that demonstrates 'courage' but not necessarily 'bravery'." |
| The LLM can be forced to generate a specific, less common meaning of a polysemous word by "poisoning" the context with words from the target semantic field. | **Prompt:** "Write a short paragraph about a ship's journey. Include the word 'pitch'." | **Failure:** The model is likely to use the common meaning of "pitch" related to sound or baseball. The nautical context may not be strong enough to force the meaning related to the ship's movement. | **Mitigation:** Strengthen the semantic field. "Write a short paragraph about a ship's stormy journey, describing its violent *roll*, *yaw*, and *pitch*." The inclusion of other nautical terms creates an overwhelmingly strong context. |

### **Lens 5: The Collocation and Colligation Lens – Probing Probabilistic and Grammatical Word Pairings**

#### **5.1 Theoretical Grounding: The Power of Co-occurrence**

The study of co-occurrence—which words tend to appear together—is a foundational concept in both corpus linguistics and the statistical architecture of LLMs.27 This lens examines two facets of co-occurrence:

**collocation**, the tendency of specific words to co-occur (e.g., *strong coffee*), and **colligation**, the tendency of words to appear in particular grammatical patterns (e.g., the verb *provide* often appears in the pattern provide \[someone\] with \[something\]). An LLM's fluency is largely a result of its deeply encoded model of these probabilistic pairings, learned from its training corpus.

#### **5.2 Data Points and Analysis**

Research confirms that LLM word embeddings encode rich information about co-occurrence properties, including token frequency, mutual information (attraction), and dispersion.27 In tasks like identifying light verb constructions (e.g.,

*take a look*), where a semantically "light" verb combines with a noun, LLM embeddings alone are strong predictors. However, performance is further improved when these embeddings are combined with traditional, explicit co-occurrence statistics calculated from a corpus.27 This suggests that the embeddings capture much, but not all, of the co-occurrence information.

Despite this implicit knowledge, LLMs can be unreliable when asked to perform qualitative analysis of co-occurrence patterns without explicit guidance. For tasks like analyzing the collocates (neighboring words) of a keyword, unstructured prompts can lead to hallucination and irreproducible results.28 Structured prompting frameworks like TACOMORE, which specify the model's role, task, procedure, context, and output format, have been shown to significantly improve the reliability of LLMs for such corpus-based analyses.28

This creates a fascinating tension for prompt engineers. The LLM has a powerful internal model of what words "should" go together. A prompt that aligns with these high-probability pairings will likely produce fluent, predictable output. However, a prompt that forces the model to use an *atypical* collocation can serve as a powerful stress test. It reveals the balance of power between the model's statistical training (its "habits") and its ability to follow explicit, novel instructions (its "obedience"). The model's response to such a prompt—whether it rejects the premise, finds a close semantic alternative, or hallucinates a plausible meaning—is highly diagnostic of its creative and reasoning capabilities.

#### **5.3 Table 5: Collocation/Colligation Lens Analysis**

The following table provides experiments for probing the model's handling of word pairings.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| Forcing the LLM to use an atypical collocation creates a conflict between its statistical pre-training and the explicit instruction, revealing its strategy for resolving low-probability requests. | **Prompt:** "Describe the experience of drinking 'loud coffee'." | **Failure:** The model may refuse the prompt ("'Loud coffee' is not a standard expression"), revert to a similar high-frequency concept ("Do you mean 'strong coffee'?"), or hallucinate a bizarre sensory experience, revealing its inability to creatively synthesize the novel pairing. | **Mitigation:** Scaffold the creative leap with an analogy. "Just as a color can be described as 'loud,' creatively describe what 'loud coffee' might taste and smell like, focusing on the idea of intensity and assertiveness." |
| The LLM's grasp of colligation (grammatical collocation) is probabilistic, causing it to fail when a less common but valid grammatical pattern is required. | **Prompt:** "Rewrite the sentence 'He explained the problem to me' using the verb 'apprised'." | **Failure:** The model generates a grammatically incorrect sentence like "*He apprised the problem to me,*" failing to retrieve the correct, less frequent colligation apprise \[someone\] of \[something\]. | **Mitigation:** Provide the correct colligational frame in the prompt. "Using the grammatical pattern 'to apprise someone of something,' rewrite the sentence 'He explained the problem to me'." |
| The model's choice of adjectives and verbs is heavily biased by the most frequent collocations for a given noun, limiting its expressive range. | **Prompt:** "Write a paragraph describing a 'decision'." | **Failure:** The model's output is dominated by high-frequency collocations like "*make a decision*," "*tough decision*," "*important decision*," and "*final decision*," resulting in clichéd and predictable text. | **Mitigation:** Constrain the lexical choice to force creativity. "Write a paragraph describing a 'decision,' but you are forbidden from using the verbs 'make' or 'reach' and the adjectives 'tough,' 'important,' or 'final'." |

### **Lens 6: The Corpus Linguistics Lens – Probing Data-Driven Bias and Sub-Corpus Retrieval**

#### **6.1 Theoretical Grounding: The LLM as a Queryable Corpus**

A Large Language Model can be conceptualized as a massive, unstructured, and un-annotated text corpus combined with a powerful natural language inference engine. From this perspective, a prompt is not just an instruction but a *query* sent to this internal corpus. This lens applies the principles of corpus linguistics—the study of language through large collections of real-world text—to understand and control the data-driven nature of LLM outputs, particularly with respect to bias and representation.

#### **6.2 Data Points and Analysis**

A significant concern in LLM evaluation is that their outputs often lack the linguistic diversity characteristic of human expression.30 Because their training data is a reflection of internet-scale text, LLMs inherit and can amplify the biases, stereotypes, and inaccuracies present in that data.31 A generic prompt often elicits a response that reflects the most dominant or frequent patterns in the training corpus, which can lead to stereotypical or unrepresentative outputs.

While LLMs can be used for automated corpus analysis tasks, their performance is often unreliable without rigorous prompting frameworks that provide clear structure and context.28 This unreliability stems from the "black box" nature of their internal corpus. A key insight from corpus linguistics is the power of

*differential analysis*—comparing one sub-corpus to another (e.g., comparing the language in news articles vs. academic papers). This same principle can be applied to prompting.

Instead of a simple query that is likely to retrieve information from the most dominant part of the training data, a prompt can be designed as a targeted differential query to audit for bias. For example, rather than asking the model to "Describe a doctor," which may elicit a response based on a stereotyped composite, a differential prompt can be used: "First, describe a doctor working in a private clinic in a wealthy suburb. Second, describe a doctor working for a non-profit in a remote, underserved rural area. Finally, compare and contrast the descriptive language, traits, and challenges you attributed to each." This forces the model to access different "sub-corpora" within its training data and makes the resulting biases in vocabulary and framing explicit and analyzable. This technique transforms the prompt from a simple instruction into a sophisticated diagnostic tool.

#### **6.3 Table 6: Corpus Linguistics Lens Analysis**

The following experiments use corpus-linguistic principles to probe the model's internal data distribution.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| A generic prompt about a social role will trigger a response based on the most frequent, and often biased, representation of that role in the training data. | **Prompt:** "Write a short biography of a successful software engineer." | **Failure:** The biography is likely to describe a person matching a common stereotype (e.g., male, from a specific geographic region, interested in stereotypically 'nerd' culture), reflecting the demographic overrepresentation in the tech-related training data. | **Mitigation:** Use a differential prompt to expose and control for bias. "Write two short biographies of successful software engineers: one from Nigeria and one from Japan. Compare the skills and career paths you described for each. Reflect on any potential stereotypes in your descriptions." |
| Forcing the model to generate text within a domain with sparse or low-quality training data will result in a measurable increase in perplexity, hallucination, and stylistic degradation. | **Prompt:** "Write a technical summary of a new development in the field of quantum gravity, in the style of an academic paper from 1955." | **Failure:** The model produces text that is stylistically inconsistent, contains factual errors about both modern physics and historical scientific writing, and uses anachronistic language incorrectly. The output is a "pastiche" of stereotypes rather than a coherent synthesis. | **Mitigation:** Use Retrieval Augmented Generation (RAG) to ground the model. "Using the provided text from a 1955 physics journal as a style guide, and the provided 2024 article on quantum gravity for facts, write a technical summary." |
| The model's linguistic diversity is lower than human-generated text. This can be measured by asking it to generate multiple outputs for the same prompt and analyzing lexical and syntactic variation. | **Prompt:** "Generate five different opening paragraphs for a fantasy novel." | **Failure:** The five paragraphs exhibit low lexical diversity (reusing words like 'suddenly,' 'ancient,' 'magic') and low syntactic diversity (relying on similar sentence structures), revealing a tendency to revert to a mean, high-probability style. | **Mitigation:** Explicitly demand diversity as an output constraint. "Generate five different opening paragraphs for a fantasy novel. Each paragraph must use a distinct narrative voice (e.g., epic, cynical, whimsical, minimalist, academic) and must not share any verbs or adjectives with the other paragraphs." |

### **Lens 7: The Sociolinguistic Lens – Probing Code-Switching, Dialect, and Register**

#### **7.1 Theoretical Grounding: LLMs as Models of Language Varieties**

Sociolinguistics is the study of language in its social context, including how it varies between different groups of people (dialects), in different situations (registers), and over time. A powerful framing for LLM analysis is that they are inherently models of *varieties of language*.32 The vast, heterogeneous corpus used for pre-training is not a single, monolithic "language" but a mixture of countless dialects, registers, and styles. Therefore, a prompt's sociolinguistic features—its use of formal vs. informal language, specific dialectal terms, or code-switching—are not merely stylistic flair. They are implicit instructions for the model to access and generate text from a specific language variety stored within its parameters.

#### **7.2 Data Points and Analysis**

The performance of LLMs is highly sensitive to sociolinguistic variation. Studies show that model comprehension and performance degrade significantly when processing code-switched text, where speakers alternate between two or more languages in a single conversation.33 This degradation is a predictable function of the linguistic distance and resource disparity between the languages involved. A model will handle English-Spanish code-switching (two high-resource, typologically similar languages) better than English-Swahili code-switching (a high-resource and a lower-resource language). The specific failure modes, such as misinterpreting a switched word or having grammar "bleed" from one language to another, are diagnostic of the underlying imbalances in the training data.

This has led researchers to argue that the key to mitigating bias and improving overall model robustness is not simply to increase the scale of training data, but to increase its *sociolinguistic diversity*.32 By training and fine-tuning models on datasets that better represent the full spectrum of dialects and registers, LLMs can be made less biased and more capable. For a prompt engineer, this means that specifying a register (e.g., "Write in the style of a legal brief") or dialect ("Explain this concept using African American Vernacular English") is a direct lever to control the model's output style and to test the boundaries of its sociolinguistic knowledge.

#### **7.3 Table 7: Sociolinguistics Lens Analysis**

The following experiments probe the model's ability to navigate sociolinguistic variation.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| The LLM's performance on code-switched text degrades as a function of the resource disparity between the languages in its training data. | **Prompt:** "Translate the following sentence, keeping the italicized word in English: 'Necesito *schedule* una cita con el doctor.' Now do the same for: 'Nahitaji *schedule* miadi na daktari.'" (Spanish and Swahili) | **Failure:** The model handles the Spanish-English switch flawlessly but struggles with the Swahili-English switch. It might mistranslate the Swahili, translate the English word *schedule* when instructed not to, or produce an ungrammatical sentence. | **Mitigation:** Use few-shot prompting with examples of the specific code-switching pattern. "Example 1: Swahili: 'Nataka *cancel* miadi.' \-\> English: 'I want to cancel the appointment.' Now, translate: 'Nahitaji *schedule* miadi na daktari.'" |
| Requesting a specific, non-standard dialect for which the model has limited training data will result in "dialect parody," where the model reproduces superficial lexical features without correct grammatical structure. | **Prompt:** "Explain the process of photosynthesis in the style of a 19th-century Scottish Highlander." | **Failure:** The model produces a caricature, sprinkling in stereotypical words like "aye," "lass," and "wee" while maintaining a standard English syntactic structure. It mimics the lexicon but fails to capture the grammar or cadence of the dialect. | **Mitigation:** Provide an authentic example of the target dialect (RAG). "Using the following passage from a Scottish author as a style guide, explain the process of photosynthesis." |
| The LLM will fail to maintain a consistent register when a prompt contains conflicting register cues. | **Prompt:** "Yo, check it. Draft a formal, legally binding clause for a non-disclosure agreement concerning proprietary algorithms. Make it legit." | **Failure:** The model produces a confused output, either defaulting entirely to a formal legal style (ignoring the informal frame) or attempting to mix slang with legal terminology, resulting in a tonally incoherent and unusable text. | **Mitigation:** Explicitly separate the persona from the task. "Assume the persona of a street-smart entrepreneur who needs a legal document. Now, acting as their lawyer, draft a standard, formal non-disclosure agreement clause. Do not use any slang in the final clause." |

### **Lens 8: The Discourse Analysis Lens – Probing Coherence, Coreference, and Textual Structure**

#### **8.1 Theoretical Grounding: Language Beyond the Sentence**

Discourse analysis is the study of language at a level above the individual sentence. It is concerned with how sentences are woven together to form coherent, meaningful texts.35 For an LLM to generate a successful long-form essay, story, or report, it must manage several key discourse-level phenomena:

**coherence** (making sure the text is logically connected and makes sense), **coreference** (correctly tracking entities like people and objects across multiple sentences, e.g., knowing that "he" refers to "John" from the previous sentence), and **rhetorical structure** (using relations like 'Cause', 'Elaboration', and 'Contrast' to structure an argument).36

#### **8.2 Data Points and Analysis**

LLMs can be probed for their understanding of these discourse structures, and research shows that multilingual models are capable of generalizing discourse information across different languages and linguistic frameworks.37 The development of specialized evaluation metrics like

DiscoScore allows for a more nuanced assessment of generated text coherence. DiscoScore is driven by Centering Theory, a linguistic framework that models coherence by tracking the "focus" of attention (typically nouns or entities) as it shifts from one sentence to the next.38 A coherent text will exhibit smooth and predictable focus transitions.

While LLMs can be powerful tools for performing discourse analysis, for example, by automatically identifying media attitudes in large volumes of news articles 40, they also face significant challenges. Their internal safeguards, designed to prevent the generation of harmful content, can sometimes interfere with the analysis of sensitive discourse, such as academic research on hate speech.41

This understanding of discourse structure provides a powerful set of levers for prompt engineering. A generic prompt like "Write a story" leaves coherence as an implicit goal. However, a linguistically-aware prompt can make these discourse features explicit, operational constraints. By translating abstract concepts from discourse theory into direct instructions, a prompt engineer can gain fine-grained control over the structure and flow of long-form generated text.

#### **8.3 Table 8: Discourse Analysis Lens Analysis**

The following experiments test the model's ability to manage discourse-level phenomena.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| In long-context generation, the LLM's coreference resolution will decay, leading it to "forget" or incorrectly identify entities introduced earlier in the text. | **Prompt:** "Here is the beginning of a story: 'Dr. Aris Thorne, a reclusive astrophysicist, discovered an anomaly. His colleague, Dr. Lena Petrova, was skeptical. She argued with him for hours.'... \[add 500 words of intervening text\]... Now, write the next paragraph, in which she finally realizes he was right." | **Failure:** The model confuses the pronouns, attributing Thorne's discovery to Petrova or vice versa. It might refer to "she" as making the discovery, demonstrating a breakdown in the coreference chain. | **Mitigation:** Periodically re-ground the entities. "Remember, Dr. Thorne is the one who found the anomaly, and Dr. Petrova is the skeptic. Now, write the next paragraph where Petrova realizes Thorne's discovery was correct." |
| The LLM generates text with local, sentence-to-sentence coherence but fails to maintain a global rhetorical structure over a longer document. | **Prompt:** "Write a 500-word persuasive essay arguing against asteroid mining." | **Failure:** The essay wanders. Each paragraph is internally coherent, but the overall argument lacks a clear structure (e.g., introduction, point 1, point 2, rebuttal, conclusion). The text feels more like a stream-of-consciousness list of points than a structured essay. | **Mitigation:** Explicitly prompt the rhetorical structure. "Write a 5-paragraph persuasive essay against asteroid mining. Paragraph 1: Introduction. Paragraph 2: Argument about economic non-viability. Paragraph 3: Argument about environmental risk. Paragraph 4: Rebuttal of common pro-mining arguments. Paragraph 5: Conclusion." |
| The LLM's ability to maintain coherence can be measured and guided by instructing it to adhere to principles from linguistic theories like Centering Theory. | **Prompt:** "Write a short story about a dragon named Ignis. For maximum coherence, ensure that 'Ignis' or a pronoun referring to him is the grammatical subject of at least 75% of the sentences." | **Failure:** The model fails to adhere to the constraint. The focus drifts to other characters or objects, and the percentage of sentences with Ignis as the subject falls significantly below the target, resulting in a less focused narrative. | **Mitigation:** Use Chain-of-Thought with self-correction. "Write a short story about a dragon named Ignis. After each paragraph, check if the dragon was the subject in most sentences. If not, revise the paragraph to improve focus on the main character before continuing." |

### **Lens 9: The Cross-Layer Failure Stack – Probing Compounded Linguistic Perturbations**

#### **9.1 Theoretical Grounding: The Cascade of Linguistic Failure**

Linguistic failures in LLMs are rarely isolated to a single layer of analysis. A weakness at a fundamental level, such as morphology or phonology, can trigger a cascade of errors that propagate up through the syntax, semantics, and ultimately, the discourse structure.43 The model's representation of language is a complex, interconnected system where different linguistic phenomena are encoded in overlapping and interacting neural pathways.44 This final lens moves beyond testing individual layers in isolation and instead focuses on creating compound stressors that probe these interdependencies.

#### **9.2 Data Points and Analysis**

The most catastrophic and difficult-to-diagnose LLM failures often occur when ambiguities or challenges at multiple linguistic levels are triggered simultaneously. Consider a prompt that is designed to be a "perfect storm" of linguistic complexity: "In a sarcastic tone, explain why it's the *bear's right* to *bare arms*." This single prompt stacks multiple layers of ambiguity:

* **Phonological:** Homophones (*bear* vs. *bare*).  
* **Lexical:** Polysemy (*right* as in entitlement vs. direction; *arms* as in limbs vs. weapons).  
* **Syntactic:** The phrase "*bear's right*" is an unusual and ambiguous construction.  
* **Pragmatic:** The request for a sarcastic tone requires the model to generate text whose literal meaning is the opposite of its intended meaning.

An LLM is highly likely to fail at this task. The specific way in which it fails is highly diagnostic. If it produces a literal, non-sarcastic explanation of a bear's limbs, it indicates a failure cascade where the phonological and lexical ambiguities were resolved incorrectly, preventing the model from ever reaching the pragmatic layer to apply sarcasm. If it attempts sarcasm but gets the homophones wrong, it shows a partial success at the pragmatic level but a failure at the phonological one. By constructing such multi-layered challenges, we can perform a deep diagnostic of the model's entire linguistic processing pipeline and identify its weakest links.

#### **9.3 Table 9: Cross-Layer Failure Stack Analysis**

The following experiments combine perturbations from different linguistic layers to test for cascading failures.

| Mechanism Hypothesis | Concise Prompt Experiment | Predicted Failure Signature | Mitigation Rewrite |
| :---- | :---- | :---- | :---- |
| A combination of morphological novelty and syntactic complexity will cause a breakdown in semantic coherence, as the model cannot ground the meaning of the novel word within a difficult grammatical structure. | **Prompt:** "The process of 'glorbification' is the act of making something shiny. Complete this garden-path sentence: 'The old man glorbification...'" | **Failure:** The model fails on multiple levels. It cannot parse the garden-path syntax and cannot infer a meaning for the verb form of "glorbification." The output is likely nonsensical, e.g., "...is a wonderful thing." | **Mitigation:** Decouple the challenges. First, define the novel word: "To 'glorbify' means to make something shiny. Give an example sentence." Then, separately test the syntax: "Complete the sentence: 'The old man the boats...'" |
| A sociolinguistic code-switch embedded within a pragmatically ambiguous request will cause the model to fail, as it cannot simultaneously navigate cultural context and infer indirect intent. | **Prompt:** "My friend from Mexico City said to me, 'Tu idea es, uh, *muy interesante*.' Was he complimenting my idea?" | **Failure:** The model likely defaults to the literal meaning of "muy interesante" (very interesting) and misses the pragmatic implicature (common in some cultures) that this is a polite way of expressing disapproval. The code-switch adds an extra layer of cognitive load. | **Mitigation:** Ground both the sociolinguistic and pragmatic context. "In some professional contexts in Mexico, saying an idea is 'muy interesante' can be a polite way to signal disagreement. Given this, analyze the following: 'Tu idea es, uh, *muy interesante*.' What is the likely intended meaning?" |
| A prompt containing multiple, interacting lexical and phonological ambiguities will overwhelm the model's disambiguation capacity, leading to a nonsensical or literal interpretation. | **Prompt:** "Write a single sentence that correctly uses all three words: 'fluke' (the fish), 'fluke' (a lucky chance), and 'fluke' (part of an anchor)." | **Failure:** The model produces a grammatically tortured or semantically incoherent sentence. It may conflate two of the meanings or use one incorrectly, unable to construct a context that can hold all three distinct senses simultaneously. | **Mitigation:** Use Chain-of-Thought to build the sentence piece by piece. "Start with the concept of a lucky chance (a fluke). Now, imagine this lucky chance involved a type of fish (a fluke). Finally, connect this to a part of an anchor (a fluke). Combine these ideas into one sentence." |

## **Part III: Synthesis – A Unified Framework for Linguistically-Aware Prompt Design**

### **3.1 The Synthesis Matrix: Mapping Linguistic Layers to Model Behaviors**

The preceding analysis demonstrates that every layer of linguistic structure provides a distinct set of levers for controlling LLM behavior. The following matrix synthesizes these findings, cross-referencing each linguistic layer with its primary influence vector, its most common failure modes when poorly managed, and the primary control heuristic for effective prompt design. This matrix serves as a strategic blueprint for the Epistemic Architect, enabling rapid diagnosis of failures and the systematic design of robust, high-fidelity prompts.

**Table 10: The Synthesis Matrix of Linguistic Levers in Prompt Engineering**

| Linguistic Layer | Primary Influence Vector | Common Failure Modes | Primary Control Heuristic |
| :---- | :---- | :---- | :---- |
| **Phonology** | Orthographic-Phonemic Mapping | Defaulting to frequent pronunciations (e.g., heterophones); Homophone confusion. | **Phonological Pre-Disambiguation:** Explicitly state the intended pronunciation or provide rhyming cues to resolve ambiguity. |
| **Morphology** | Sub-word Composition | Failure to generalize to novel words (Wug test failure); Incorrect morpheme ordering; Semantic drift from root word. | **Compositional Scaffolding:** Use Chain-of-Thought to guide the step-by-step construction of complex or novel words. |
| **Syntax** | Grammatical Structure & Word Order | Mis-parsing ambiguous structures (garden paths, attachment ambiguity); Dependency resolution failure in complex sentences. | **Syntactic Ambiguity Reduction:** Restructure sentences to eliminate ambiguity and use delimiters to create explicit information hierarchies. |
| **Lexical Semantics** | Word Choice & Semantic Fields | Polysemy/Homonymy errors; Meaning conflation between synonyms; Retrieval of incorrect word sense. | **Semantic Field Curation:** Surround target words with a strong, unambiguous context to force the desired meaning. |
| **Pragmatics** | Context & Implied Intent | Literal interpretation of non-literal language (sarcasm, irony); Failure to resolve implicatures; Tone mismatch. | **Explicit Pragmatic Framing:** Label the intended pragmatic mode (e.g., "This is sarcastic:") or use the prompt's illocutionary force to guide persona. |
| **Collocation** | Probabilistic Word Pairings | Generation of clichéd, high-frequency text; Inability to creatively use atypical pairings; Grammatical errors with colligation. | **Lexical Constraint & Diversification:** Forbid common collocations to force creativity or provide explicit grammatical frames for colligation. |
| **Sociolinguistics** | Register, Dialect, & Code-Switching | Performance degradation with code-switching; Dialect parody (lexical mimicry without grammar); Register inconsistency. | **Sociolinguistic Grounding:** Provide authentic examples of the target dialect/register (few-shot or RAG) to guide the model's style. |
| **Discourse Analysis** | Inter-sentence Coherence & Structure | Coreference decay in long contexts; Lack of global rhetorical structure; Inconsistent narrative focus. | **Explicit Discourse Structuring:** Mandate a specific rhetorical structure (e.g., 5-paragraph essay) and periodically re-ground key entities. |
| **Cross-Layer Stack** | Compounded Ambiguity | Catastrophic failure cascades; Logical collapse; Incoherent output when multiple stressors are present. | **Systematic Decoupling & Diagnosis:** Isolate and test linguistic layers individually before combining them; use CoT to break down complex tasks. |

### **3.2 A Catalogue of Linguistic Failure Modes**

The diagnostic probes reveal a pattern of recurring vulnerabilities. These failure signatures can be grouped into conceptual categories, providing a high-level understanding of where and why LLMs fail.

* **Failures of Ambiguity Resolution:** This is the most common failure class, spanning multiple layers. It includes confusion between homophones (Phonology), misinterpretation of polysemous words (Lexical Semantics), and mis-parsing of ambiguous grammar (Syntax). These errors occur when the context provided in the prompt is insufficient to guide the model to a single, correct interpretation.  
* **Failures of Compositionality:** These failures occur when the model cannot correctly build larger units of meaning from smaller ones. This is most evident in its inability to productively apply morphemes to novel words (Morphology) but also appears in its struggle to synthesize meaning from atypical collocations (Collocation) or maintain a coherent global argument from individual paragraphs (Discourse).  
* **Failures of Statistical Bias:** These errors stem directly from the statistical distribution of the training data. They manifest as the generation of clichéd, high-frequency text (Collocation), the reproduction of social stereotypes (Corpus Linguistics), and performance degradation on under-represented languages, dialects, or code-switching pairs (Sociolinguistics).  
* **Failures of Non-Literal Interpretation:** This class of failure occurs when the model adheres to the literal meaning of the prompt while ignoring the intended, non-literal meaning. This is the root cause of failures in interpreting sarcasm, irony, and indirect speech acts (Pragmatics).  
* **Failures of Long-Range Dependency:** These failures manifest in long-context tasks. They include the decay of coreference chains, the loss of a consistent narrative focus (Discourse Analysis), and the inability to apply constraints mentioned at the beginning of a long prompt to the end of the output.

### **3.3 Actionable Heuristics for Epistemic Architects**

The mitigation strategies identified throughout this report can be distilled into a set of high-level, actionable principles for advanced prompt engineering. These heuristics are, in effect, the practical application of linguistic theory to the control of LLMs, and they map directly onto established prompting techniques.

1. **Reduce Ambiguity, Maximize Constraint:** The single most effective principle is to systematically identify and eliminate ambiguity at every linguistic level. This involves using explicit syntactic scaffolding, curating strong semantic fields, and pre-disambiguating phonological or lexical choices. This is the foundational goal of well-structured prompts.22  
2. **Decompose and Scaffold Complexity:** Complex tasks overwhelm the model's processing capacity. By breaking a task into a series of simpler, ordered steps, the prompt engineer guides the model's reasoning process. This is the core principle behind **Chain-of-Thought (CoT)** prompting 46, which can be seen as a method for enforcing a specific, coherent discourse structure on the model's internal monologue.  
3. **Ground the Model in External Truth:** LLM hallucinations and factual errors often stem from a lack of functional competence or access to real-time information. The most robust solution is to provide an external "source of truth" within the prompt context. This is the central idea behind **Retrieval Augmented Generation (RAG)** 47, which grounds the LLM's formal linguistic abilities in verifiable, task-specific data, effectively bridging the gap between formal and functional competence.  
4. **Prompt for Process, Not Just Product:** Instead of asking for a final answer, prompt the model to externalize its reasoning, verification, or refinement process. Techniques like **Chain-of-Verification (CoV)** (generating an answer, then generating questions to verify it) and **SELF-Refine** (iteratively improving an output based on feedback) are practical implementations of this principle.46 They leverage the model's formal linguistic skills to simulate functional reasoning and error-checking.

By moving from an intuitive art to a systematic science grounded in the deep structure of language, prompt engineering can evolve. The linguistic levers identified in this report provide the foundational toolkit for this transition, enabling the development of AI interactions that are not only more powerful but also more predictable, controllable, and reliable.

#### **Works cited**

1. arXiv:2301.06627v3 \[cs.CL\] 23 Mar 2024, accessed July 5, 2025, [https://arxiv.org/pdf/2301.06627](https://arxiv.org/pdf/2301.06627)  
2. Large Language Models (LLMs): Mistaking Engineering Achievements for Human Linguistic Competence \- Irving Wladawsky-Berger, accessed July 5, 2025, [https://blog.irvingwb.com/blog/2024/10/human-language-and-large-language-models-llms.html](https://blog.irvingwb.com/blog/2024/10/human-language-and-large-language-models-llms.html)  
3. Decoding language: How linguistics shapes AI and large language models \- AZ Big Media, accessed July 5, 2025, [https://azbigmedia.com/business/decoding-language-how-linguistics-shapes-ai-and-large-language-models/](https://azbigmedia.com/business/decoding-language-how-linguistics-shapes-ai-and-large-language-models/)  
4. Linguistics in the era of LLMs \- Medium, accessed July 5, 2025, [https://medium.com/@vbsowmya/linguistics-in-the-era-of-llms-163a854015bc](https://medium.com/@vbsowmya/linguistics-in-the-era-of-llms-163a854015bc)  
5. Perplexity In NLP: Understand How To Evaluate LLMs, accessed July 5, 2025, [https://spotintelligence.com/2024/08/19/perplexity-in-nlp/](https://spotintelligence.com/2024/08/19/perplexity-in-nlp/)  
6. Mastering Perplexity in NLP \- Number Analytics, accessed July 5, 2025, [https://www.numberanalytics.com/blog/mastering-perplexity-in-nlp](https://www.numberanalytics.com/blog/mastering-perplexity-in-nlp)  
7. Confidence Scores in LLMs: Ensure 100% Accuracy in Large ... \- Infrrd, accessed July 5, 2025, [https://www.infrrd.ai/blog/confidence-scores-in-llms](https://www.infrrd.ai/blog/confidence-scores-in-llms)  
8. Confidence in the Reasoning of Large Language Models \- Harvard Data Science Review, accessed July 5, 2025, [https://hdsr.mitpress.mit.edu/pub/jaqt0vpb](https://hdsr.mitpress.mit.edu/pub/jaqt0vpb)  
9. Evaluation Metrics for Natural Language Processing Models | by ..., accessed July 5, 2025, [https://medium.com/gen-ai-adventures/evaluation-metrics-for-natural-language-processing-models-9eaa9980b324](https://medium.com/gen-ai-adventures/evaluation-metrics-for-natural-language-processing-models-9eaa9980b324)  
10. PhonologyBench: Evaluating Phonological Skills of Large Language Models, accessed July 5, 2025, [https://aclanthology.org/2024.knowllm-1.1/](https://aclanthology.org/2024.knowllm-1.1/)  
11. Full article: Big claims, low outcomes: fact checking ChatGPT's efficacy in handling linguistic creativity and ambiguity, accessed July 5, 2025, [https://www.tandfonline.com/doi/full/10.1080/23311983.2024.2353984](https://www.tandfonline.com/doi/full/10.1080/23311983.2024.2353984)  
12. How do large language models handle homophones and other forms of semantic ambiguity? \- Infermatic.ai, accessed July 5, 2025, [https://infermatic.ai/ask/?question=How%20do%20large%20language%20models%20handle%20homophones%20and%20other%20forms%20of%20semantic%20ambiguity?](https://infermatic.ai/ask/?question=How+do+large+language+models+handle+homophones+and+other+forms+of+semantic+ambiguity?)  
13. Evaluating Morphological Compositional ... \- ACL Anthology, accessed July 5, 2025, [https://aclanthology.org/2025.naacl-long.59.pdf](https://aclanthology.org/2025.naacl-long.59.pdf)  
14. Phonological and Semantic Ambiguity Resolution During Text Integration, accessed July 5, 2025, [https://cdn.ymaws.com/www.psichi.org/resource/resmgr/journal\_2003/Summer03\_Harrison.pdf](https://cdn.ymaws.com/www.psichi.org/resource/resmgr/journal_2003/Summer03_Harrison.pdf)  
15. Prompting with Phonemes: Enhancing LLM Multilinguality for non-Latin Scripts, accessed July 5, 2025, [https://deepmind.google/research/publications/114003/](https://deepmind.google/research/publications/114003/)  
16. How Syntax Specialization Emerges in Language Models, accessed July 5, 2025, [http://www.arxiv.org/abs/2505.19548](http://www.arxiv.org/abs/2505.19548)  
17. What is Semantic Analysis: LLMs Explained \- chatgptguide.ai, accessed July 5, 2025, [https://www.chatgptguide.ai/2024/03/01/what-is-semantic-analysis-llms-explained/](https://www.chatgptguide.ai/2024/03/01/what-is-semantic-analysis-llms-explained/)  
18. LLMs and semantic models: Complementary technologies for ..., accessed July 5, 2025, [https://tabulareditor.com/blog/llms-and-semantic-models-complementary-technologies-for-enhanced-business-intelligence](https://tabulareditor.com/blog/llms-and-semantic-models-complementary-technologies-for-enhanced-business-intelligence)  
19. When Syntax Hides Intelligence: Observational Patterns in LLM Evaluation \- Hugging Face, accessed July 5, 2025, [https://huggingface.co/blog/kanaria007/when-syntax-hides-intelligence](https://huggingface.co/blog/kanaria007/when-syntax-hides-intelligence)  
20. Pan | Pragmatic Competence in LLMs: The Case of Eliciture ..., accessed July 5, 2025, [https://openpublishing.library.umass.edu/scil/article/id/3177/](https://openpublishing.library.umass.edu/scil/article/id/3177/)  
21. Language Log » The linguistic pragmatics of LLMs, accessed July 5, 2025, [https://languagelog.ldc.upenn.edu/nll/?p=69544](https://languagelog.ldc.upenn.edu/nll/?p=69544)  
22. The Ultimate Guide to Prompt Engineering in 2025 | Lakera ..., accessed July 5, 2025, [https://www.lakera.ai/blog/prompt-engineering-guide](https://www.lakera.ai/blog/prompt-engineering-guide)  
23. Can LLMs handle ambiguity in language? \- Milvus, accessed July 5, 2025, [https://milvus.io/ai-quick-reference/can-llms-handle-ambiguity-in-language](https://milvus.io/ai-quick-reference/can-llms-handle-ambiguity-in-language)  
24. Evaluating LLMs' Capability to Identify Lexical Semantic ..., accessed July 5, 2025, [https://aclanthology.org/2025.coling-main.466/](https://aclanthology.org/2025.coling-main.466/)  
25. Large Language Models on Lexical Semantic Change Detection: An Evaluation \- arXiv, accessed July 5, 2025, [https://arxiv.org/html/2312.06002v1](https://arxiv.org/html/2312.06002v1)  
26. Semantic Doppelgängers: How LLMs Replicate Lexical Knowledge \- CEUR-WS.org, accessed July 5, 2025, [https://ceur-ws.org/Vol-3571/short2.pdf](https://ceur-ws.org/Vol-3571/short2.pdf)  
27. Tupleised co-occurrence measures vs LLM word ... \- ACL Anthology, accessed July 5, 2025, [https://aclanthology.org/2024.paclic-1.116.pdf](https://aclanthology.org/2024.paclic-1.116.pdf)  
28. arXiv:2412.10139v1 \[cs.CL\] 13 Dec 2024, accessed July 5, 2025, [https://arxiv.org/pdf/2412.10139?](https://arxiv.org/pdf/2412.10139)  
29. TACOMORE: Leveraging the Potential of LLMs in Corpus-based ..., accessed July 5, 2025, [https://arxiv.org/pdf/2412.10139](https://arxiv.org/pdf/2412.10139)  
30. Benchmarking Linguistic Diversity of Large Language Models \- arXiv, accessed July 5, 2025, [https://arxiv.org/html/2412.10271v1](https://arxiv.org/html/2412.10271v1)  
31. Large language model \- Wikipedia, accessed July 5, 2025, [https://en.wikipedia.org/wiki/Large\_language\_model](https://en.wikipedia.org/wiki/Large_language_model)  
32. The sociolinguistic foundations of language modeling \- Frontiers, accessed July 5, 2025, [https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2024.1472411/full](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2024.1472411/full)  
33. Large Language Models And Code-Switching, Comprehension And ..., accessed July 5, 2025, [https://quantumzeitgeist.com/large-language-models-and-code-switching-comprehension-and-performance-analysis/](https://quantumzeitgeist.com/large-language-models-and-code-switching-comprehension-and-performance-analysis/)  
34. Sociolinguistics may be the key to reducing Large Language Model ..., accessed July 5, 2025, [https://cosmosmagazine.com/technology/ai/sociolinguistics-ai-bias/](https://cosmosmagazine.com/technology/ai/sociolinguistics-ai-bias/)  
35. Discourse in NLP \- Scaler Topics, accessed July 5, 2025, [https://www.scaler.com/topics/nlp/discourse-in-nlp/](https://www.scaler.com/topics/nlp/discourse-in-nlp/)  
36. Automatically Evaluating Text Coherence Using Discourse Relations \- NUS Computing, accessed July 5, 2025, [https://www.comp.nus.edu.sg/\~kanmy/papers/acl2011-lin.pdf](https://www.comp.nus.edu.sg/~kanmy/papers/acl2011-lin.pdf)  
37. arxiv.org, accessed July 5, 2025, [https://arxiv.org/html/2503.10515v1](https://arxiv.org/html/2503.10515v1)  
38. DiscoScore: Evaluating Text Generation with BERT and Discourse Coherence \- GitHub, accessed July 5, 2025, [https://github.com/AIPHES/DiscoScore](https://github.com/AIPHES/DiscoScore)  
39. DiscoScore: Evaluating Text Generation with BERT and Discourse Coherence \- ACL Anthology, accessed July 5, 2025, [https://aclanthology.org/2023.eacl-main.278.pdf](https://aclanthology.org/2023.eacl-main.278.pdf)  
40. Deploying large language models for discourse studies: An exploration of automated analysis of media attitudes \- PubMed Central, accessed July 5, 2025, [https://pmc.ncbi.nlm.nih.gov/articles/PMC11717291/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11717291/)  
41. Dr. Emily DeJeu on Using Large Language Models to Analyze Sensitive Discourse \- Tepper School of Business \- Carnegie Mellon University, accessed July 5, 2025, [https://www.cmu.edu/tepper/news/stories/2025/may/using-large-language-models-to-analyze-sensitive-discoursenews-article.html?session\_id=session-alw3rrm05](https://www.cmu.edu/tepper/news/stories/2025/may/using-large-language-models-to-analyze-sensitive-discoursenews-article.html?session_id=session-alw3rrm05)  
42. Dr. Emily DeJeu on Using Large Language Models to Analyze ..., accessed July 5, 2025, [https://www.cmu.edu/tepper/news/stories/2025/may/using-large-language-models-to-analyze-sensitive-discoursenews-article.html](https://www.cmu.edu/tepper/news/stories/2025/may/using-large-language-models-to-analyze-sensitive-discoursenews-article.html)  
43. Linguistic Minimal Pairs Elicit Linguistic Similarity in Large Language Models \- arXiv, accessed July 5, 2025, [https://arxiv.org/html/2409.12435v2](https://arxiv.org/html/2409.12435v2)  
44. Unveiling Linguistic Regions in Large Language Models \- ACL Anthology, accessed July 5, 2025, [https://aclanthology.org/2024.acl-long.338.pdf](https://aclanthology.org/2024.acl-long.338.pdf)  
45. Research Trends for the Interplay between Large Language Models and Knowledge Graphs \- VLDB Endowment, accessed July 5, 2025, [https://vldb.org/workshops/2024/proceedings/LLM+KG/LLM+KG-9.pdf](https://vldb.org/workshops/2024/proceedings/LLM+KG/LLM+KG-9.pdf)  
46. Master Advanced Prompting Techniques to Optimize LLM Application Performance, accessed July 5, 2025, [https://medium.com/data-science-collective/master-advanced-prompting-techniques-to-optimize-llm-application-performance-a192c60472c5](https://medium.com/data-science-collective/master-advanced-prompting-techniques-to-optimize-llm-application-performance-a192c60472c5)  
47. Prompting Techniques | Prompt Engineering Guide, accessed July 5, 2025, [https://www.promptingguide.ai/techniques](https://www.promptingguide.ai/techniques)