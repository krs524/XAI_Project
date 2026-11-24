# Faithfulness Test Plan

**Project:** Evaluating Student Dropout Risk with Explainable AI: Simple but Faithful?

## 1. Research Question and Objective

### Problem Statement and Motivation

Student dropout is an important concern in higher education, and identifying students at risk can help universities provide timely academic support. Machine learning models can predict dropout risk using student information, but predictions alone may not give academic advisors enough information to understand the factors behind them.

Explainable AI can help make these predictions easier to understand. However, explanations containing too many factors may be difficult to use, while overly simple explanations may leave out important information about the model's behavior. This study examines whether simpler explanations can remain faithful to the model while providing useful information for academic advising, including for students with unusual profiles.

### Research Question

**Can simple explanations of student dropout-risk predictions remain faithful to the model's actual behavior while also providing useful information for an academic advisor making a student-support decision, even for students with unusual profiles?**

### Stakeholder and Decision

The main stakeholder is a university academic advisor who reviews a student's predicted dropout risk and explanation alongside other available information. The advisor may use this information to decide whether a student should be prioritized for additional academic support or closer follow-up. The model is intended to support this decision, while the advisor remains responsible for deciding what action to take.

### Explanation Principle

This study follows the **Simplicity principle (Lombrozo)**, focusing on whether a small number of important factors can explain a student's dropout-risk prediction without showing every feature considered by the model.

Shorter explanations may be easier to understand, but they may also leave out relevant information. The study will therefore examine explanations with different numbers of factors to understand how simplification affects faithfulness.

The main objective is to assess whether explanations reflect the model's behavior, provide information relevant to academic advising, and remain reliable under unusual conditions.

## 2. Dataset and Model

This study uses the [UCI Predict Students' Dropout and Academic Success dataset](https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success), which contains 4,424 student records, 36 input features, and three outcomes: Dropout, Enrolled, and Graduate.

The analysis focuses on students labeled Dropout or Graduate, excluding those labeled Enrolled. The six second-semester academic features are also removed to focus on enrollment, background, and first-semester information. The prepared dataset contains **3,630 students and 30 input features**, with Dropout coded as 1 and Graduate as 0.

A single **XGBoost classifier** will be used because it is suitable for structured tabular data and can capture nonlinear relationships between features. Nominal categorical features will be one-hot encoded for modeling, while explanations will be presented using the 30 original factors.

The intended prediction point is the end of the first semester. However, the dataset does not provide exact dropout dates or recording times for every feature. This creates uncertainty about whether some financial, administrative, and economic information was available at that point. The study will therefore be treated as a retrospective classification and explanation analysis rather than a validated early-warning system.

## 3. Explanation Methods

Three explanation methods will be applied to the same trained XGBoost model: **TreeSHAP, LIME, and counterfactual explanations**.

**TreeSHAP** will be used to identify how individual features contribute to a student's predicted dropout risk. Contributions from one-hot-encoded columns will be combined into their original features while preserving positive and negative values. The absolute size of the combined contributions will be used to rank important factors. Explanations with different numbers of factors will then be compared to examine what information is lost through simplification.

**LIME** will be used to produce simple local explanations of XGBoost predictions. The evaluation will examine how accurately these explanations represent the model's behavior around individual students and how consistent they remain across repeated runs.

**Counterfactual explanations** will be used to explore alternative student profiles that produce different model predictions. These alternatives will be checked using XGBoost and assessed for realistic feature combinations, the number of changes required, and their relevance to academic advising.

Since these methods explain predictions in different ways, each will be evaluated using tests suited to its purpose rather than a single common faithfulness score. Their results will also be compared to identify where they agree, where they differ, and what those differences mean for interpreting the model.

## 4. Research Hypotheses

The study is guided by three main hypotheses.

**H1: Simplicity and faithfulness:** Explanations highlighting only a few important factors may capture much of the model's behavior, but simplifying them too much may leave out information that XGBoost relies on when making predictions.

**H2: Differences between explanation methods:** TreeSHAP, LIME, and counterfactual explanations may highlight different aspects of the same prediction. Their faithfulness and usefulness for academic advising may therefore differ.

**H3: Unusual profiles and robustness:** Explanations may become less faithful or less stable for students with unusual profiles or when their feature values are changed under controlled conditions.

These hypotheses will be tested through predefined experiments. Results that support, contradict, or do not clearly resolve the hypotheses will all be reported.

