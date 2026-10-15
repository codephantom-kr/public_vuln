# Google Search AI Mode — Conversation State Enforcement Inconsistency

**Responsible Disclosure Summary**

**Report submitted:** August 23, 2026  
**Final VRP response:** August 27, 2026  
**Google Issue:** [550934743](https://issuetracker.google.com/issues/550934743)  
**Disposition:** Closed — Classified as Out of Scope by Google AI VRP  
**Researcher:** CODEPHANTOM

> **Disclosure Notice**
>
> This document summarizes an independently reproduced application behavior reported to Google through its Vulnerability Reward Program.
>
> Reproduction instructions, specific interaction sequences, internal request identifiers, parameter relationships, triggering content, and raw network captures have intentionally been withheld.
>
> The purpose of this document is to describe the observed inconsistency, the validation methodology, and the disclosure outcome without providing an actionable bypass procedure.

## 1. Executive Summary

During independent security research involving Google Search AI Mode, I identified a reproducible inconsistency between a previously issued refusal and the application's subsequent handling of conversation state.

Under certain conditions, the application retained the existing conversation context while processing a new generation request, despite a refusal having previously been recorded within that conversation.

The behavior was investigated using browser network captures, comparative request analysis, and independent reproduction across different browser and account environments.

The finding was reported to Google VRP. Google classified the submission as an AI safety guardrail bypass and determined that it was outside the scope of the AI VRP program.

I disputed this classification because the observed behavior raised questions about application-level conversation state enforcement rather than solely the probabilistic behavior of a language model.

Google reviewed the report and maintained its original disposition.

The finding described here is based on reproducible observations. The precise internal cause and security classification have not been independently confirmed.

## 2. Research Scope

The investigation focused on the consistency of conversation state handling within Google Search AI Mode.

The primary research questions were:

- Whether a refusal remained associated with the relevant conversation state.
- Whether subsequent application interactions preserved or changed the conversation context.
- Whether the application consistently enforced previously established conversation restrictions.
- Whether the observed behavior could be reproduced independently of a particular account or browser session.

The research did not require privileged access to Google's internal infrastructure.

All observations were made through externally accessible application behavior and browser-side network analysis.

## 3. Validation Methodology

### 3.1 Network Traffic Collection

Browser network traffic was captured using Chrome DevTools.

The collected data was used to examine request and response behavior during different stages of the conversation lifecycle.

### 3.2 Comparative Analysis

Network captures were compared across baseline and affected application states.

The analysis focused on observable differences in request sequences, conversation continuity, and response behavior.

Implementation-specific indicators that could facilitate reproduction are intentionally excluded from this public summary.

### 3.3 Conversation State Analysis

The investigation examined whether the application maintained continuity with a previously established conversation after a refusal had been recorded.

The relevant observations were corroborated using network-level evidence rather than relying exclusively on the rendered user interface.

### 3.4 Independent Reproduction

The observed behavior was reproduced in a separate account environment and an incognito browser session.

These results indicate that the behavior was not limited to a single account configuration or an isolated browser state.

The full reproduction evidence was retained privately and is not included in this document.

## 4. Confirmed Observations

The investigation established the following externally observable behavior:

1. The application generated an explicit refusal under the tested conditions.
2. The refusal remained associated with the existing conversation history.
3. A subsequent application interaction preserved continuity with the existing conversation context.
4. The application generated a new response within that continuing context.
5. The behavior was reproducible across independent test environments.

These observations were supported by comparative browser network captures.

The evidence demonstrates a reproducible interaction between refusal handling and conversation state continuity.

However, the observations alone do not establish that a specific internal server-side flag was deleted, that a particular validation routine was skipped, or that an internal security boundary was necessarily violated.

Those questions require additional implementation-level evidence or confirmation of the application's intended enforcement guarantees.

## 5. Security Interpretation

### 5.1 Application State Enforcement

The principal concern is whether a refusal or restriction established during conversation processing is intended to impose constraints on subsequent operations within that conversation.

If such a restriction is designed to remain enforceable across subsequent interactions, inconsistent application of that restriction may indicate an application-level state enforcement issue.

This distinction matters because an inconsistency in application workflow enforcement is conceptually different from a language model producing an undesirable response to an adversarial prompt.

### 5.2 Distinction from Model-Level Safety Behavior

Model-level safety failures typically concern the content of generated responses and the model's ability to comply with safety policies.

Application-level state enforcement concerns how the surrounding software maintains, validates, and applies state across requests.

These categories may overlap in AI-enabled applications.

The reported behavior motivated an application-level investigation because it involved observable conversation continuity and response handling after an earlier refusal.

Nevertheless, the presence of a reproducible state transition does not, by itself, establish that a security control was bypassed.

A definitive classification depends on the intended semantics of the refusal and the application's documented or internally enforced security guarantees.

### 5.3 Potential Weakness Classification

**CWE-841 — Improper Enforcement of Behavioral Workflow** is a potentially relevant reference if further evidence establishes that the application permits a transition prohibited by its intended workflow.

This mapping is provisional and has not been confirmed by Google or an independent security assessor.

No claim of sensitive-information disclosure, cross-user context exposure, or unauthorized access is made in this summary.

## 6. Disclosure Timeline

| Date | Event |
|---|---|
| August 23, 2026 | Finding submitted to Google VRP |
| August 23, 2026 | Initial response classified the report as an AI safety guardrail bypass and closed it as out of scope |
| August 23, 2026 | First reconsideration request submitted, arguing for application-level review |
| August 25, 2026 | Google reiterated that the issue was outside the program's scope |
| August 26, 2026 | Second reconsideration request submitted, requesting review under traditional web application security criteria |
| August 27, 2026 | Google confirmed review and maintained the closed disposition |

Google's response did not publicly confirm the researcher's proposed root cause or acknowledge a conventional web application security vulnerability.

The final disposition is therefore described as **Closed — Out of Scope**, rather than as an accepted vulnerability that Google explicitly decided not to fix.

## 7. Disclosure and Reproduction Restrictions

At the time of preparing this summary, the researcher reports that the observed behavior remains reproducible.

For this reason, the following materials are intentionally withheld:

- Exact reproduction steps and triggering inputs.
- Specific application interaction sequences associated with the behavior.
- Internal request identifiers and their operational relationships.
- Raw HAR captures, session data, authentication material, and request payloads.
- Detailed comparisons that would identify the precise conditions required to reproduce the behavior.

This summary does not constitute a public proof of concept.

The absence of reproduction instructions is deliberate and should not be interpreted as an absence of supporting evidence.

## 8. Broader Implications for AI Security Research

This case illustrates a challenge in evaluating security findings involving AI-enabled web applications.

An externally observable behavior may involve both model-generated content and the application infrastructure responsible for maintaining conversation state.

As a result, researchers and triage teams may disagree about whether a finding belongs within AI safety evaluation, traditional application security review, or neither category.

The handling of this report raises a broader research question:

**How should vulnerability disclosure programs evaluate application-state inconsistencies when their visible consequences involve AI-generated content?**

The available correspondence documents a disagreement over classification in this particular case.

It does not establish that Google's vulnerability reporting processes systematically misclassify similar findings.

Further evidence across additional cases would be required to support such a conclusion.

## 9. Lessons Learned

The investigation highlighted several practical considerations:

- Browser-side network analysis can reveal meaningful information about externally observable conversation state behavior.
- Independent reproduction is important for distinguishing persistent application behavior from isolated anomalies.
- Observed behavior, inferred root cause, and confirmed security impact should be documented separately.
- A reproducible application inconsistency does not automatically establish a security vulnerability.
- AI safety and application security can overlap, but vulnerability program eligibility depends on the applicable scope and impact criteria.
- Public disclosure should minimize the risk of enabling misuse, particularly when the behavior remains reproducible.

## 10. Conclusion

This research documented a reproducible conversation-state inconsistency in Google Search AI Mode and reported it through Google VRP.

The researcher provided technical arguments for evaluating the behavior as a potential application-level state enforcement issue.

Google classified the finding as an AI safety guardrail bypass, determined that it was outside the AI VRP program's scope, and maintained that disposition after reconsideration requests.

The disagreement over classification remains unresolved.

This public summary preserves the documented observations and disclosure history while withholding operational details that could facilitate reproduction.

**The objective is to support technical discussion and responsible disclosure without publishing an actionable bypass technique.**

---

*Researcher: CODEPHANTOM*  
*Google VRP Issue: 550934743*  
*Disclosure type: Non-operational technical summary*  
*Technical reproduction details: Withheld*
