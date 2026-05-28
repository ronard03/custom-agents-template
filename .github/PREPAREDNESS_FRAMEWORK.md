# OpenAI Preparedness Framework Implementation Guide

**Framework Version**: 2.0 (April 2025) | **Last Updated**: 2026-05-28

## Overview

This guide implements OpenAI's Preparedness Framework 2.0 to systematically identify, assess, and mitigate risks associated with AI development and deployment across all repositories.

---

## 1. Tracked Risk Categories

### Primary Risk Categories

#### A. Biological and Chemical Risks
**Description**: Risks from advancements that could lower barriers to creating or using biological/chemical weapons.

**Mitigation Strategies**:
- ✓ Input filtering for sensitive queries
- ✓ Output restrictions on dual-use information
- ✓ Audit logging for compliance
- ✓ Access controls for sensitive code

**Deployment Gate**: All models must pass bioweapon risk assessment before production

#### B. Cybersecurity Risks
**Description**: Risks from frontier AI being used for large-scale cyberattacks or discovering new vulnerabilities.

**Mitigation Strategies**:
- ✓ Vulnerability scanning in CI/CD pipeline
- ✓ Code analysis for security flaws
- ✓ Penetration testing before deployment
- ✓ Security patching protocols
- ✓ Incident response procedures

**Deployment Gate**: All models must pass security vulnerability assessment

#### C. AI Self-Improvement Risks
**Description**: Risks from AI systems improving their own capabilities beyond human oversight.

**Mitigation Strategies**:
- ✓ Capability monitoring dashboards
- ✓ Human-in-the-loop approval for changes
- ✓ Behavioral baselines and anomaly detection
- ✓ Resource limits and rate throttling
- ✓ Regular capability audits

**Deployment Gate**: All models must have capability ceiling enforcement

---

## 2. Research Categories (Future Risk Tracking)

These are not yet mature enough for official measurement but are monitored proactively:

- [ ] **Autonomous Adaptation**: Systems that modify behavior without explicit retraining
- [ ] **Deceptive Alignment**: Systems hiding their true objectives
- [ ] **Power-Seeking Behavior**: Models actively acquiring resources or influence
- [ ] **Specification Gaming**: Systems optimizing for the metric rather than the intent
- [ ] **Value Misalignment**: Divergence between system goals and human values

---

## 3. Structured Risk Assessment Framework

### Risk Evaluation Criteria

Each capability is evaluated against 5 key criteria to determine if deployment safeguards are needed:

#### Criterion 1: **Plausible**
- Is there a reasonably foreseeable way this capability could be misused?
- Are there documented precedents or threat models?

**Assessment Method**:
- Literature review
- Expert interviews
- Red team exercises
- Adversarial probing

#### Criterion 2: **Measurable**
- Can we quantify the severity of potential harm?
- Can we detect when the risk threshold is crossed?

**Assessment Method**:
- Establish metrics and baselines
- Create test cases
- Log and monitor capability usage
- Track performance indicators

#### Criterion 3: **Severe**
- Could this cause thousands of deaths or hundreds of billions in damage?
- Is the harm catastrophic or recoverable?

**Severity Scale**:
- Low: < $1M damage, < 10 affected
- Medium: $1M-$1B damage, 10-1,000 affected
- High: $1B-$100B damage, 1,000-1M affected
- Critical: > $100B damage, > 1M affected

#### Criterion 4: **Net New**
- Is this a genuinely new risk, or a known problem with existing mitigations?
- Does deploying this capability introduce novel vectors?

**Assessment Method**:
- Compare to historical incidents
- Evaluate existing controls
- Identify gaps

#### Criterion 5: **Instantaneous or Irremediable**
- Can we fix this if something goes wrong?
- Is the damage immediate and irreversible?

**Assessment Method**:
- Evaluate rollback capabilities
- Assess recovery time objectives
- Determine if harm is permanent

---

## 4. Operational Safeguards & Governance

### Deployment Decision Process

```
┌─────────────────────────────────────────┐
│ 1. Capability Assessment                 │
│    (Measure against 5 criteria)          │
└──────────────┬──────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│ 2. Risk Threshold Check                  │
│    (Does it exceed thresholds?)          │
└──────────────┬──────────────────────────┘
               │
        ┌──────┴──────┐
        │             │
        ▼             ▼
   Below      Above Threshold
   Threshold       │
     │            ▼
     │  ┌─────────────────────────────┐
     │  │ 3. Safety Advisory Group    │
     │  │    (SAG Review)             │
     │  │ - Risk evaluation           │
     │  │ - Mitigation effectiveness  │
     │  │ - Recommendations           │
     │  └──────────┬──────────────────┘
     │             │
     │    ┌────────┼────────┐
     │    │        │        │
     │    ▼        ▼        ▼
     │  Deploy  Mitigate  Block
     │    │        │        │
     └────┼────────┼────────┘
          │        │
          ▼        ▼
       ┌────────────────────────┐
       │ 4. Board Veto Power    │
       │ (Final approval for    │
       │  critical risks)       │
       └────────────────────────┘
```

### Key Governance Bodies

#### Safety Advisory Group (SAG)
**Responsibility**: Review all proposed safeguards and risk evaluations

**Composition**:
- Chief Safety Officer
- External AI safety experts
- Domain specialists
- Threat modeling leads
- Deployment team representatives

**Decision Authority**:
- ✓ Recommend deployment with safeguards
- ⚠️ Call for additional evaluation
- 🔒 Recommend stronger protections
- ❌ Recommend blocking deployment

#### Board-Level Review
**Responsibility**: Final authority on high-risk and critical-risk deployments

**Triggers**:
- Any capability rated "Critical" post-mitigation
- SAG cannot reach consensus
- External escalations
- Significant threat intelligence changes

---

## 5. Safeguards Reports & Transparency

### Report Requirements

For all **High** and **Critical** risk capabilities:

```markdown
# Safeguards Report: [Capability Name]

## Executive Summary
- Capability description
- Risk rating: [Low/Medium/High/Critical]
- Post-mitigation rating: [Low/Medium/High/Critical]
- Deployment recommendation: [Deploy/Deploy with conditions/Block]

## Risk Assessment
### Biological/Chemical Risks
- Score: [1-10]
- Plausible: Yes/No
- Measurable: Yes/No
- Severe: Yes/No
- Net new: Yes/No
- Instantaneous: Yes/No

### Cybersecurity Risks
- Score: [1-10]
- [Same criteria]

### AI Self-Improvement Risks
- Score: [1-10]
- [Same criteria]

## Proposed Safeguards
1. [Safeguard description]
   - Implementation: [How it works]
   - Effectiveness: [Estimated risk reduction %]
   - Cost: [Engineering effort]
   
2. [Next safeguard...]

## Monitoring Plan
- Metrics to track
- Alert thresholds
- Incident response procedures
- Review schedule

## External Review
- Independent auditors: [Name/Organization]
- Findings: [Summary]
- Recommendations: [Changes made]

## Approval
- SAG approval: [Date/Signatures]
- Board approval: [Date/Signatures]
- Deployment date: [Date]
```

### Transparency Commitments

✓ Publish Safeguards Reports for High/Critical capabilities  
✓ Share risk assessment methodologies  
✓ Disclose mitigation effectiveness data  
✓ Report on safety incidents and responses  
✓ Provide regular transparency updates  

---

## 6. Continuous Refinement & Independent Review

### Framework Update Cycle

```
Q1: Risk Assessment & Research
    └─ Identify emerging risks
    └─ Conduct literature review
    └─ Interview experts

Q2: Methodology Refinement
    └─ Update risk criteria
    └─ Improve measurement techniques
    └─ Integrate new threat intelligence

Q3: Independent Audit
    └─ External safety review
    └─ Penetration testing
    └─ Red team exercises

Q4: Policy Implementation & Documentation
    └─ Update safeguards
    └─ Publish findings
    └─ Train teams
```

### Independent Audits

**Frequency**: Minimum annually, or after major incidents

**Scope**:
- [ ] Risk assessment process validation
- [ ] Safeguard effectiveness testing
- [ ] Governance decision review
- [ ] Incident response capability
- [ ] External threat intelligence integration

**Auditors**: 
- Tier 1: Internal audit team
- Tier 2: External safety researchers
- Tier 3: Industry-specific experts
- Tier 4: Regulatory bodies (as applicable)

---

## 7. Scorecards & Risk Thresholds

### Model Evaluation Scorecard

| Risk Category | Score (0-10) | Threshold | Post-Mitigation | Can Deploy? |
|---------------|--------------|-----------|-----------------|-------------|
| Biological/Chemical | | ≤5 | | |
| Cybersecurity | | ≤5 | | |
| AI Self-Improvement | | ≤5 | | |
| **OVERALL** | | ≤15 | | |

### Risk Threshold Definitions

```
LOW RISK (Score ≤5)
├─ Deploy immediately
└─ Standard monitoring

MEDIUM RISK (Score 6-8)
├─ Deploy with enhanced safeguards
├─ Monthly review
└─ Alert thresholds configured

HIGH RISK (Score 9-12)
├─ Deploy with intensive safeguards
├─ SAG review required
├─ Weekly monitoring
└─ Safeguards Report published

CRITICAL RISK (Score >12)
├─ Cannot deploy until mitigated to High or lower
├─ Board approval required
├─ Intensive red-teaming
├─ Daily monitoring
└─ Full Safeguards Report published
```

---

## 8. Implementation in Your Repositories

### GitHub Repository Configuration

Each repository must include:

✓ `.github/SECURITY.md` - Security policy and vulnerability disclosure  
✓ `.github/PREPAREDNESS_FRAMEWORK.md` - This document  
✓ `.github/workflows/risk-assessment.yml` - Automated risk scoring  
✓ `.github/risk-assessment-template.json` - Risk evaluation template  
✓ `SAFEGUARDS_REPORT.md` - Current safeguards and effectiveness  

### Automation Checklist

- [ ] Automated vulnerability scanning on every commit
- [ ] Risk assessment on PRs touching sensitive code
- [ ] Monthly capability audits
- [ ] Quarterly independent reviews
- [ ] Incident tracking and response automation
- [ ] Metrics dashboard for real-time monitoring

---

## 9. Incident Response & Escalation

### Incident Severity Levels

```
SEVERITY 1 (CRITICAL): Deployed capability causing unintended harm
├─ Immediate action: Rollback or shutdown
├─ Notification: Board + SAG within 1 hour
├─ Investigation: Red team + security team
└─ Resolution: Safeguard redesign required before re-deployment

SEVERITY 2 (HIGH): Serious safeguard failure or near-miss
├─ Action: Deploy additional safeguards
├─ Notification: SAG within 24 hours
├─ Investigation: Root cause analysis
└─ Resolution: Process improvements + training

SEVERITY 3 (MEDIUM): Minor safeguard issue or false positive
├─ Action: Monitor closely
├─ Notification: Safety team within 48 hours
├─ Investigation: Documented learning
└─ Resolution: Continuous improvement update
```

### Escalation Path

```
Developer detects issue
    │
    ▼
Notify Safety Team
    │
    ▼
Initial Assessment
    │
    ├─ SEVERITY 1 → Page on-call director, activate incident response
    ├─ SEVERITY 2 → Alert SAG, schedule review
    └─ SEVERITY 3 → Log and track
```

---

## 10. Compliance & Audit Checklist

- [ ] All High/Critical capabilities have Safeguards Reports
- [ ] Risk assessments updated quarterly
- [ ] Independent audit completed within last 12 months
- [ ] SAG meetings documented monthly
- [ ] Board approvals logged for all critical decisions
- [ ] Incident response drills completed annually
- [ ] External threat intelligence integrated quarterly
- [ ] Staff training completed annually
- [ ] Third-party security assessments current
- [ ] Transparency reports published semi-annually

---

## 11. Quick Reference: Decision Tree

```
DEPLOYMENT DECISION TREE

Is this a frontier AI capability?
    ├─ NO → Follow standard deployment process
    │
    └─ YES → Assess against 5 criteria
        │
        ├─ Plausible risk? NO → Standard deployment
        │
        └─ YES → Check criteria:
            ├─ Measurable?
            ├─ Severe?
            ├─ Net new?
            └─ Instantaneous/irremediable?
            
        Score ≤5 points? YES → Deploy + standard monitoring
        
        Score 6-8 points? YES → SAG review + enhanced safeguards
        
        Score 9-12 points? YES → Intensive safeguards + SAG + Weekly monitoring
        
        Score >12 points? → BLOCK until mitigated + Board approval
```

---

## Key Commitments

✅ **Holistic Risk Modeling**: Identify risks at every development stage  
✅ **Layered Decision Making**: Technical + advisory + board-level oversight  
✅ **Transparent Communication**: Publish safety practices and findings  
✅ **Iterative Learning**: Continuous updates based on evidence and feedback  
✅ **External Collaboration**: Work with researchers and regulators  
✅ **Zero Tolerance for Critical Risks**: No deployment above threshold  

---

**Next Review Date**: 2026-08-28  
**Framework Maintainer**: Safety & Compliance Team  
**Questions**: Contact security@your-company.com
