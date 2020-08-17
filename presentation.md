class: center, middle

# I can't believe it's still here!

## Gavin Chan

## Principal Quant Developer, AXA IM Chorus Ltd


---

# My Background

- AXA IM Chorus Ltd is a quantitative asset management firm.

- Develop in Python

- Research in Python

- Manage the portfolio in Python

---

# Case study - Knight Capital Group

.right[
<img src="out/1200px-Knight_Capital_Group_logo.svg.gif" width=30%>
]

- Knight Capital Group was the largest trader in U.S. equities in 2012

- Market making more than 19,000 US securities in more than 21 billion dollars average daily volume

- A technical incident on August 1, 2012 blew up the company

- The incident costed Knight $440 million

- Finally, Knight was acquired by a rival, Getco LLC

---

# Background of the event

- The largest market Knight works on is the public market / exchange

- Knight competes with the financial services providing private markets / dark pools

- Since 2008, the dark pool trading rose from 15% to more than 40% of all stock trades

- In 2011, the NYSE proposed Retail Liquidity Program (RLP), its own dark pool

- After NYSE received SEC approval in end June, it quickly announced the RLP goes live 
in 1st August and Knight decided to participate the programme.

- Software development team needed to make changes on the existing trade execution system, Smart Market Access Routing System (SMARS), to support RLP, in 30 days

---

# Coding Horror

- SMARS routed the parent orders to the downstream execution between dozens of different trading venues

- The new RLP code in SMARS replaced some unused code, an order algorithm called "Power Peg", of the order router

- The algorithm "Power Peg" was not used in the live, production environment, but in a controlled environment for testing

- Power Peg, designed to buy high and sell low,  was already a "dead code" in the RLP deployment

- The new RLP code reuses a flag which activated the Power Peg previously. When the flag is set to "yes",
it activated the RLP functionality in the new SMARS but the Power Peg in the old SMARS

---

# Deployment Chaos

- In the week before go-live, an engineer manually deployed the new RLP code in SMARS to 7 out of 8 production servers

- However, the missing one was neither alerted or notified by other engineers

---

# Production Jungle

- On 1st August, 8:01 a.m., 97 emails of alerts "Power Peg disabled" were sent to the Knight personnel, but not as high-priority alerts and not reviewed by the staff

- At 9.30 a.m., RLP orders from broker-dealers were routed to Knight and the eighth server activated with the Power Peg algorithm started to continously send child orders, neglecting the parent orders

- By 9.34 a.m., NYSE noticed the doubled trading volume originated from Knight and alerted the CIO

- However, they could neither flipped the kill switch nor identified the root cause immediately 

- Until 9.58 a.m., the root cause was identified and the server was shut down.

---

# Nightmare

- Executed nearly 400 million shares with a net long position of $3.5 billion and a net short position of $3.15 billion.

- SEC refused to cancel most of the bogus trades and Knight had to settle those trades

- Knight had to close out the net positions and in estimate it costed Knight around $440 million

- A week later, Knight received $400 million cash infusion from investors. In end 2012, Knight was bought by Getco and combined as KCG Holdings.

---

# Lesson to learn

- Regularly clean up the dead code

> a) Use version control smartly, for example in git

>> `git diff -G<keyword>`

>> `git log --after <date> --until <date>`

> b) Devote time for debt reduction and refactoring

- Automated Testing and Test automation

---

# Hard to get the stains off?

- Uncertainty and undefinite business value to clean technical debt

- Unclear usage from the clients and downstream processes on the existing application

- Missing tools / systematic approach to remove unused codes

---

# Approach - Planning and Warning

- Alerts users and developers on the deprecation plan

- Requires planning ahead on the support timeline

- Documentation / Enhancement proposal can help draw the timeline

.center[
<img src="out/nep_29.png" width=70%>
###### [NEP-29 Recommend Python and Numpy version support as a community policy standard](https://numpy.org/neps/nep-0029-deprecation_policy.html)
]

- [Semantic Versioning](https://semver.org/) - MAJOR version when you make incompatible API changes


---

# Approach - Expired and Cleaning

- Expired stage throws an exception on the application level if the function is called

- Cleaning stage removes the deprecation part from the source code

---

# Auto-deprecator

.center[
<img src="out/cycle.png" width=90%>
]

---

# Auto-deprecator - Warning

- Specify the current version and the target expired version

```python
from auto_deprecator import deprecate

@deprecate(expiry='2.0.0', current='1.9.0')
def old_hello_world():
    return print("Hello world!")
```

```bash
(bash) hello-world-app
Hello world!
DeprecationWarning: The function "old_hello_world" will be deprecated in version 2.0.0
```

---

# Auto-deprecator - Warning


- Specify the current version by the package version and the migrated function

```python
from auto_deprecator import deprecate

from hello_world import __version__

@deprecate(expiry='2.0.0', current=__version__, relocate='hello_world')
def old_hello_world():
    return 'hello world'
```

---

# Auto-deprecator - Expired

- Test the expired stage scenario

```bash
(bash) DEPRECATED_VERSION=2.0.0 hello-world-app
Traceback (most recent call last):
 ...
 RuntimeError: The function "old_hello_world" is deprecated in version 2.0.0
```

---

# Auto-deprecator - Cleaning

- Command `auto-deprecate` removes the deprecated function from the source code

.center[

```
$ auto-deprecate hello_world.py --version 2.1.0
```

<img src="out/auto_deprecate_diff.png" width=90%>

<img src="out/auto_deprecate_diff_comment.png" width=90%>

]

---

# Frequent rewrites

.center[
"Most software at Google gets rewritten every few years"

"Software that is a few years old was designed around an older set of requirements and is
typically not designed in a way that is optimal for current requirements."

"Rewriting code cuts away all the unnecessary accumulated complexity that was addressing requirements which are no longer so important"
]

.right[
#### [Software Engineering at Google, Fergus Henderson](https://arxiv.org/ftp/arxiv/papers/1702/1702.01715.pdf)
]

---

### Reference

- https://www.henricodolfing.com/2019/06/project-failure-case-study-knight-capital.html

---

class: center, middle

### If you are interested to the project __auto-deprecator__, please visit [https://github.com/auto-deprecator/auto-deprecator](https://github.com/auto-deprecator/auto-deprecator)

### Github: [@gavincyi](https://github.com/gavincyi)

### Email: [gavincyi@gmail.com](https://bit.ly/2YFSm3F)

### Linkedin: [Gavin, Ying In Chan](https://www.linkedin.com/in/gavin-ying-in-chan-43b00127/)

