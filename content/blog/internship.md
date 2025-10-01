---
title: 'Inside AWS: Lessons from My Internship'
date: 2025-09-01
draft: false
description: 'Real insights from building production systems at Amazon Web Services'
slug: 'aws-internship'
tags: ['AWS', 'Internship', 'Software Engineering', 'Career']
---

Three months at Amazon Web Services taught me more about real software engineering than two years of coursework. Not because school isn't valuable—it absolutely is—but because building systems that handle millions of dollars in enterprise contracts hits different than implementing data structures for homework.

Here's what I learned during my internship on the EC2 Private Pricing team, where I built an automated workflow system to extend AWS's enterprise discount system from US-only to international markets.

## The Reality of Big Tech Engineering

### Two-Pizza Teams Actually Work

Amazon's famous "two-pizza team" rule isn't just corporate folklore. My team had eight people, and that small size meant:

- Direct access to senior engineers and principal architects
- Ownership of end-to-end features
- Quick decision-making without layers of bureaucracy
- Everyone knew what everyone else was working on

The flip side? No hiding. Your contributions (or lack thereof) are immediately visible. There's nowhere to coast.

### The Development Rhythm

**Daily Standups (10 minutes, actually enforced)**
"What I did yesterday, what I'm doing today, any blockers." That's it. No status reports, no lengthy discussions. Blocked? We'd handle it after standup.

**Two-Week Sprints**
Sprint planning on Monday, retrospective and demo on Friday of week two. The predictability helped me plan my work and manage expectations—both my manager's and my own.

**Code Reviews: The Real Education**
Every single line of production code needed review from a senior engineer. Initially intimidating, it became the most valuable part of my internship. Comments like:

- "This works, but have you considered the latency impact at scale?"
- "What happens when DynamoDB throttles this request?"
- "Let's add metrics here—we'll want to monitor this in production"

These weren't nitpicks. They were lessons in thinking at AWS scale.

## Designing for the Real World

### My First Design Review Disaster

Week 2: I presented my initial design—a straightforward solution for handling Indian marketplace pricing. I'd spent days on it, feeling confident.

The feedback was brutal but constructive:

- "What about when we expand to Brazil?"
- "How does this handle currency fluctuations?"
- "What's the rollback strategy if this fails?"
- "Where are the metrics and alarms?"

I hadn't thought about any of that. School projects succeed if they work. Production systems need to work, scale, fail gracefully, recover automatically, and integrate with existing infrastructure.

### The Pivot That Changed Everything

Week 4: Business requirements changed. Instead of just India, we needed to support any future international marketplace. My specific solution became useless overnight.

This is where my manager taught me a crucial lesson: "Design for change. The only constant at AWS is that requirements will evolve."

### The Second Design: Building for Tomorrow

Armed with feedback and a broader scope, I took a different approach:

**Before jumping to solutions, I asked:**

- What problem are we actually solving?
- Who are our customers (internal and external)?
- What does success look like in metrics?
- What are the non-negotiables (latency, availability, security)?

**The new design principles:**

- Configuration over code (new marketplaces via config, not code changes)
- Modular components (each piece does one thing well)
- Comprehensive monitoring (know about issues before customers do)
- Gradual rollout capability (test with small traffic percentages)

**The stakeholder strategy:**
Instead of waiting for the formal review, I scheduled informal chats with:

- The principal engineer (architecture patterns)
- The operations team (deployment and monitoring)
- The security team (compliance requirements)
- Sister teams (integration points)

By the time the formal review came, I'd already addressed 90% of potential concerns. The review became a refinement session, not a redesign session.

## The Code Quality Awakening

### School Code vs Production Code

**School:**

```python
def calculate_discount(price, discount_rate):
    return price * (1 - discount_rate)
```

**Production:**

```python
def calculate_discount(
    price: Decimal,
    discount_rate: Decimal,
    currency: str,
    customer_id: str,
    marketplace: str
) -> PricingResult:
    """
    Calculate customer-specific discount for a given marketplace.

    Args:
        price: Base price in specified currency
        discount_rate: Decimal between 0 and 1
        currency: ISO 4217 currency code
        customer_id: Unique customer identifier
        marketplace: Target marketplace code

    Returns:
        PricingResult containing final price and audit metadata

    Raises:
        InvalidPriceError: If price is negative or exceeds limits
        InvalidDiscountError: If discount rate outside valid range
        MarketplaceNotSupportedError: If marketplace not configured
    """
    # Validate inputs
    _validate_price(price, currency, marketplace)
    _validate_discount_rate(discount_rate)
    _validate_marketplace_support(marketplace)

    # Log for audit trail
    logger.info(f"Calculating discount for customer={customer_id}, "
                f"marketplace={marketplace}, base_price={price}")

    # Calculate with proper decimal precision
    final_price = price * (Decimal('1') - discount_rate)

    # Emit metrics
    metrics.record_pricing_calculation(
        marketplace=marketplace,
        customer_segment=_get_customer_segment(customer_id),
        discount_applied=float(discount_rate)
    )

    return PricingResult(
        final_price=final_price,
        currency=currency,
        calculation_timestamp=datetime.utcnow(),
        audit_id=str(uuid.uuid4())
    )
```

The difference? Production code assumes everything can and will go wrong.

### Testing: The Unsung Hero

School projects: "It works on my machine!"
AWS: "Here's proof it works everywhere, always."

I wrote:

- Unit tests (every function, every edge case)
- Integration tests (components working together)
- Load tests (what happens at 1000 requests/second?)
- Failure tests (what if DynamoDB is down?)
- Rollback tests (can we revert safely?)

The test code was 3x longer than the actual code. That's normal.

## Time Management in the Real World

### The Milestone Mindset

My manager introduced me to working backwards from deadlines:

- Week 12: Code complete
- Week 11: Final testing and documentation
- Week 10: Code review and revisions
- Week 8-9: Implementation
- Week 6-7: Prototype and early feedback
- Week 3-5: Design and review
- Week 1-2: Onboarding and understanding context

This wasn't a suggestion—it was survival. Missing internship deadlines means no return offer.

### Managing Blockers

**The 30-Minute Rule**: Stuck for 30 minutes? Reach out. Pride has no place when you're on a deadline.

My escalation path:

1. Check documentation and code
2. Ask team chat
3. Direct message a teammate
4. Schedule time with my mentor
5. Escalate to manager

I initially felt bad about asking questions. My mentor's perspective: "You're an intern. Not asking questions would be weird."

## The AI Revolution I Didn't Expect

Amazon's internal AI tools transformed how I worked. Not ChatGPT—something more powerful because it understood Amazon's codebase, patterns, and standards.

**Understanding existing code:**
"Explain what this Lambda function does and how it interacts with DynamoDB"
_Gets detailed explanation with sequence diagrams_

**Writing new code:**
"Generate a Lambda handler that processes SNS messages for price updates, following team conventions"
_Gets production-ready code with error handling, logging, and metrics_

**Debugging:**
"This DynamoDB query is timing out in production but works locally. What could cause this?"
_Gets list of likely causes ranked by probability_

The productivity boost was insane. Tasks that would've taken hours took minutes. But here's the key: AI accelerated my learning, it didn't replace it. I still needed to understand why the AI's suggestions worked and when they didn't apply.

## The Soft Skills Nobody Talks About

### Communication Is Code

Writing clear Slack messages, emails, and documentation was as important as writing code. The template that worked:

**Context**: What are you working on?
**Problem**: What specific issue are you facing?
**What you've tried**: Shows you've put in effort
**Specific ask**: What exactly do you need?

### Managing Up

Weekly 1:1s with my manager weren't status updates—those were in written reports. Instead:

- Strategic questions about approach
- Career development discussions
- Feedback on what I could improve
- Understanding team/company priorities

### The Feedback Loop

Mid-internship review revealed I was too heads-down:
"Your code is solid, but we don't see you engaging with the broader team."

The fix: I started attending optional architecture reviews, asking questions in team meetings, and sharing what I learned in team knowledge-sharing sessions.

## The Seattle Experience

Beyond work, Seattle was incredible. The AWS office had everything—free food, gym, game rooms—but what mattered was the people. My team grabbed lunch together daily, debating everything from system design to the best coffee shops.

Weekends meant exploring Pike Place Market, hiking Rattlesnake Ledge, and yes, visiting the original Starbucks (overrated, but you have to do it).

## The Lasting Impact

This internship changed my perspective on software engineering:

1. **Scale matters**: A solution that works for 10 users might collapse at 10,000
2. **Code is communication**: Others will read, modify, and depend on your code
3. **Design for change**: Requirements will evolve, plan for it
4. **Metrics over feelings**: "I think it's fast" vs "P99 latency is 47ms"
5. **Ownership mindset**: You own your code in production, not just until merge

## Final Thoughts

The internship wasn't just about learning AWS services or writing production code. It was about understanding how world-class engineering organizations operate, how to think systematically about problems, and how to build solutions that scale.

Most importantly, it confirmed what I suspected: I love this work. The complexity, the scale, the impact—building systems that power the internet is incredibly fulfilling.

To future interns: embrace the discomfort of not knowing things, ask questions shamelessly, and remember that everyone—even principal engineers—was once where you are.
