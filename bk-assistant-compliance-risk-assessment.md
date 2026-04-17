**Jose Ruiz-Vazquez**  
*AI Governance & Risk*

**Summary of Compliance Risk for the BK Assistant Algorithm**

March 2026  |  Version 1.0

# **Overview of Algorithmic System**

BK Assistant (“Patty”) is an OpenAI-powered employee monitoring system currently piloting at approximately 500 Burger King locations in the United States, with documented deployments in Chicago and Schaumburg, Illinois.¹ The system uses voice-processing technology integrated into drive-through headsets to monitor employee speech in real time. It detects courteous language (“please,” “thank you”), tone, and adherence to scripted hospitality behaviors, then compiles per-shift “friendliness scores” surfaced to managers. Key stakeholders include Burger King Corporation, franchisee operators, shift managers, and the front-line employees whose speech is continuously monitored and scored.

# **Compliance Risk: BIPA Exposure Compounded by Minor Employee Governance Gap**

## **The BIPA Exposure**

Illinois’s Biometric Information Privacy Act (740 ILCS 14\) prohibits any private entity from collecting biometric identifiers, including voiceprints, without first obtaining a written release and maintaining a publicly available retention and destruction policy.² The statute carries a private right of action with liquidated damages of $1,000–$5,000 per violation, with no requirement that plaintiffs demonstrate actual harm.³

BK Assistant collects voice data from employees and associates derived scores with individual workers, a functional voiceprint linkage even if the system does not perform speaker identification in the traditional sense. There is no publicly available BIPA-compliant retention and destruction policy documented for BK Assistant, and the pilot locations are operating in the jurisdiction that has produced more BIPA litigation than any other in the country. A directly analogous precedent exists: in *Carpenter v. McDonald’s*, a plaintiff alleged BIPA violations arising from voice recognition technology at a McDonald’s drive-through, the identical operational context.⁴

## **The Minor Employee Governance Gap**

Food and beverage serving employs more 16-19 year-olds than any other occupation in the U.S.⁵ Burger King franchisees employ workers as young as 14 under FLSA exemptions for restaurant work. BIPA’s written consent requirement applies uniformly regardless of age, and there is no publicly documented policy establishing whether BK’s franchisees obtain separate parental consent before collecting voice data from minor employees, or whether minors are excluded from AI-monitored shifts. This creates a structural risk class: a system collecting voice-derived behavioral data in a high-litigation jurisdiction, in an industry that employs large numbers of minors, with no documented evidence of age-differentiated governance.⁶

# **Mitigation Strategy**

**1\. BIPA-Compliant Consent Process.** Implement a standalone written consent and disclosure form, separate from general onboarding paperwork, specifically describing what voice data is collected, how it is used, how long it is retained, and when it is destroyed. This is established compliance practice following *Rosenbach v. Six Flags*.

**2\. Age-Differentiated Governance.** Either require verifiable parental consent before including workers under 18 in AI-monitored shifts, or exclude minor employees from the monitoring scope entirely pending legal review.

**3\. Voiceprint Classification Review.** Conduct a formal legal analysis of whether the system’s voice-derived, per-employee scoring constitutes a “voiceprint” under BIPA’s statutory definition and current case law, and assess whether Age-Appropriate Design Code (AADC) legislation in pilot states creates additional obligations.

# **Final Thoughts**

BK Assistant is not a hypothetical future risk, it is a live system operating today in Illinois, the most active BIPA enforcement jurisdiction in the country, where the same technology in the same operational context at a direct competitor has already been the subject of litigation. The EU AI Act’s prohibition on workplace affect-inference AI (Article 5(1)(f)), effective February 2025, signals the international regulatory trajectory.⁷ The mitigation steps above are established practice; what is notable is the absence of publicly documented evidence that the standard steps have been taken. That gap is correctable, and correcting it early is materially less costly than litigating it later.

**References**

¹ NACS; ABC 7 Chicago; Central Illinois Proud — reporting on BK Assistant pilot locations including Chicago and Schaumburg, IL.

² Illinois Biometric Information Privacy Act, 740 ILCS 14/1 et seq. (2008).

³ Rosenbach v. Six Flags Entertainment Corp., 2019 IL 123186 (Ill. 2019).

⁴ Carpenter v. McDonald’s Corp., No. 21-cv-5938 (N.D. Ill.).

⁵ U.S. Bureau of Labor Statistics, Occupational Outlook Handbook, Food and Beverage Serving Occupations; Black Box Intelligence / Restaurant Dive, Q3 2021\.

⁶ Drobac, J.A., “Developing Capacity: Adolescent Consent at Work, at Law, and in the Sciences of the Mind,” Journal of Juvenile Law & Policy, Vol. 10, No. 1 (2006).

⁷ EU Artificial Intelligence Act, Article 5(1)(f), Regulation (EU) 2024/1689, effective February 2, 2025\.
