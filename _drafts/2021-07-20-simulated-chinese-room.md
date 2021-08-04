---
title: "Machines of Meaning"
excerpt: "The Simulated Chinese Room and Machine Understand"
excerpt_separator: "<!--more-->"
sidebar: false
comments: true
related: true
header:
  overlay_color: "#000"
  overlay_filter: "0.6"
  overlay_image: /assets/images/abstract_learning.png
classes: wide
categories:
  - philosophy
  - thought experiment
  - meaning
  - semantics 
  - determinism
---

One of the goals of _Artificial Intelligence_ and _Computational Semantics_ in
particular, is to learn meaningful representations for natural language
expressions. One issue with many computational methods is that they take the
philosophically challenging relationship between language and meaning and
abstract it away in problematic ways. In particular, in the field of AI, the
&mdash; implicit &mdash; hope is that by resorting to the formal and
quantitative nature of computational approaches, one can bypass the problem of
what meaning is whilst making use of shallow metaphors to justify research
directions towards ["Strong"
AI](https://plato.stanford.edu/entries/artificial-intelligence/#StroVersWeakAI){:target="_blank"}:
the view that an appropriately programmed computer is a mind, in the sense that
it can understand and have cognitive states like humans.

Coincidentally, the best examples of what we consider minds presently are based
on carbon, but this is not an absolute metaphysical necessity. The view that it
is abstract causal structure that is essential to mentality has been an implicit
assumption of the AI field since its inception, but articulated explicitly by
Putnam (1960),#TODO

Moreover, terms used to motivate particular research directions in AI are often
rooted in a human frame of reference, but omitting the philosophical character
of intentional discourse and mental terms like "belief", "understanding",
"representations", leads these concepts to be epistemically sterile. While the
focus of AI has always been on empirical success, the field is as much technical
as it is a philosophical venture, and seeing philosophical practice as a
technical matter can lead us to better interpretation of experimental outcomes
as well as open new research avenues.

I want to clarify the relationship between existing philosophical concepts and
current practices in AI. In this first post, I revisit the widely known Chinese
Room Argument and construct a characterization of meaning and intentionality
that does not depend on a human frame of reference.

## The Chinese Room Argument (CRA)

In 1980 John Searle published "[Minds, Brains and
Programs](http://cogprints.org/7150/1/10.1.1.83.5248.pdf)" where an argument and
thought experiment now generally known as "Chinese Room Argument" was put
forward:

>Suppose that I'm locked in a room and given a large batch of Chinese writing.
>Suppose furthermore (as is indeed the case) that I know no Chinese, either
>written or spoken... To me, Chinese writing is just so many meaningless
>squiggles.
>
>Now suppose further that after this first batch of Chinese writing I am given a
>second batch of Chinese script together with a set of rules for correlating the
>second batch with the first batch. The rules are in English, and I understand
>these rules as well as any other native speaker of English....Now suppose also
>that I am given a third batch of Chinese symbols together with some
>instructions, again in English, that enable me to correlate elements of this
>third batch with the first two batches, and these rules instruct me how to give
>back certain Chinese symbols with certain sorts of shapes in response to
>certain sorts of shapes given me in the third batch.
>
>Unknown to me, the people who are giving me all of these symbols call the
>first batch "a script," they call the second batch a "story. ' and they call
>the third batch "questions." Furthermore, they call the symbols I give them
>back in response to the third batch "answers to the questions." and the set of
>rules in English that they gave me, they call "the program."

Searle assumes that for the purposes of producing Chinese, he is simply an
instantiation of a computer program. But from an external point of view --from
the point of view of someone reading the answers to the Chinese questions
outside the room-- answers are equally good to answers he gives to English
questions (in which he is fluent).

This is the scenario under which Searle examines the claims made from a Strong
AI perspective:

1. the programmed computer understands the stories;
2. the program in some sense explains human understanding.

A **first conclusion** of the argument is that a computer may make it appear to
understand language but could not produce real understanding. Hence the “Turing
Test” is inadequate. Searle argues that the thought experiment underscores the
fact that computers merely use syntactic rules to manipulate symbol strings, but
have no understanding of semantics. Searle suggests that minds must result from
biological processes and computers can at best simulate these biological
processes.



### Replies to CRA

There have been numerous replies to the CRA among which: a systems-view reply
tackles the fact that while the person in the room might not know Chinese, the
person is but a part of a larger system and thus, while the man might not
understand Chinese, the system as a whole does. (See [][][].)
<!--
In response to this, Searle changes the thought experiment by getting rid of the
room, and instead internalizing all the instructions by rote learning. He will
still not understand Chinese, and this time there is nothing to the system
beyond Searle, leaving no room for the systems reply. but the problem still
persists

it is not a case of multiple personality disorder because one can follow instructions
that are not meaningful in any way yet they produce chinese 
this is infeasible because searle cannot hold everything in his head but consider 
a different scenario that illustrates where this response fails, the program is now a simulation
of a room with a person which speaks both chinese and english

another scenario is when we simulate the same room, can we attribute
intentionality to the english speaking virtual searle but not the chinese
speaking searle using instructions, I'd argue that yes, not all instructions are
the same, if we attribute intentionality to the simulation...


my view here is that a component is not what understands, it is the series of 
states that the system undergoes and the process that allows these states to occur 
that is what we call understanding
-->

<!--
‘computational theory of mind’ (CTOM), which holds that consciousness arises as
soon as the right kinds of algorithms or computations are implemented,
regardless of the material substrate, be it a biological nervous system or an
electronic computer, or something else.


TODO the man in the room is but a component, one could argue that a biological mind is 
a biological system, but our argument here is that the conceptualization of the state is 
irrelevant, the program is the mind, in the same sense that the simulation is a mind
-->
one argument might claim that the instructions are understandable but do not
create meaning, by reading the conversions instructions, the man in the room
cannot acquire understanding because of the levels of abstraction we might use
to "denote" (cite article on denotation) things in the world the whole point of
language is to abstract away details to convey information hinging on the fact
that humans are embedded in the world in a similar fashion, (that is also why
cultural differences can cause misunderstandings)

labelling the components and the rules to compute state of the universe (math is
language after all) does not mean you will instantly create these maps... you
could possibly learn the meaning over time... but the room doesn't consider this
scenario

%TODO  the justification for the computer to be implemented in pen and paper is
% given in "Views into the Chinese room: new essays on Searle and Artificial
Intelligence" % Turing gave an argument for this

TODO brief counter arguments

On the simulated brain reply, Searle agrees that it would indeed be reasonable
to attribute understanding to such an android system – but only as long as you
don’t know how it works. As soon as you know the truth – it is a computer,
uncomprehendingly manipulating symbols on the basis of syntax, not meaning – you
would cease to attribute intentionality to it.

So the issue is not whether or not something can be said to be intentional, but
whether we would be willing to attribute intentionality to it. This underlies
even more the recognition of human limitations on whether or not it is possible
to determine intentionality.

## The Simulated Room

Suppose that we take the room where Searle is locked-up in and replicate not the
mechanisms of a brain firing, but the entire room with Searle in it to the
atomic level. This of course assumes that the universe is deterministic and does
away with the current practical implications of implementing such a simulator.
The question now becomes, would the Searle outside the room reject the
intentionality of the simulated room on the basis of the system not actually
existing? After all, they are indistinguishable for the purposes of their
interaction with the environment outside the room. Would the simulated Searle
doubt it's own intentionality if told the truth about the room?

<img src="/assets/images/machine_chat.png" width="75%" alt="" class="align-center">

1. if one accepts that the universe is computable then we can construct a
   simulation down to the atomic level of an individual outside the room that
   knows both english and mandarin and

2. assuming we can build the simulation as a similar room, the output can
   interact with Searle and output clear instructions in english on how to
   change each symbol

3. ignoring the practical aspects of the endeavour, this computation can be
   performed by hand by Searle inside the room which computes the requests for
   the individual to perform the requested translation along with all the
   transition states

4. Searle still doesn't understand mandarin, and from an external perspective,
   his mandarin answers are as good as the english ones.

Therefore the objection is not so much to the set of formal instructions to
manipulate symbols, but the nature of these instructions. Now all instructions
are the same, and in this case, simulating the individual that knows how to
translate is the key factor in the success of the translation. It's state, the
result of it's experiences.

Searle would probably argue that you can attribute intentionality to such system
(as he did in the case of) but as soon as you know the truth you know it's not
the real thing. But if he accepts this scenario (predicated on the universe
being computable) then there's no basis for distinguishing a true intentional
system from a simulated one. For I can turn to Searl and say, well actually, you
are in the simulation we created to prove a point.

The point of this thought experiment is not only to poke holes in Searle's
Chinese Room thought experiment, but to highlight the limitations of what can be
said about intentional systems in general and in particular, about semantics.

The Chinese Room argument was constructed in the 80's and as such, I believe it
was taken to be a biproduct of the embrace of logic positivism in the field of
AI. While technically still dealing with computability of certain concepts, one
could argue that the state of the art in AI also influences heavily the
philosophical landscape around these issues. Current state of the art systems
are capable of highlighting human biases in a much stronger fashion due to their
capacity to do just what Searle accused these systems of doing.

<!--this is a feedback look after all -->  

Modern language models can, in effect, manipulate symbols to produce natural
language expressions which are understandable to humans, devoid of any embodied
experiential context to ground them, capable of influencing human behaviour of
humans precisely because we cannot possibly distinguish between the origins of
discourse in environments like the Web, where we have an overload of information
reducing our cognitive bandwidth and inner critique that guards against
confirmation bias.

On another note, this does not mean that models of language do not understand,
but it does imply that they can't understand like we do precisely because of how
they ground human language.

<!-- wittgensteinian meaning as use, language captures aspects of the world, language is not just optimization but game theoretic scenarios of 
expectation resolution, in the case of language models, their goals are hard coded in the objectives -->  





## References

1. Searle, J., 1980, ‘[Minds, Brains and Programs](http://cogprints.org/7150/1/10.1.1.83.5248.pdf)’, Behavioral and Brain Sciences, 3: 417–57

2. Rey, G. (1986). What's really going on in Searle's “Chinese room”. Philosophical Studies, 50(2), 169-185.

3. Rey, G. (2002). Searle’s misunderstandings of functionalism and strong AI. Views into the Chinese room: New essays on Searle and artificial intelligence, 201-225.

4. Turing, A. (1980). Computing Machinery and Intelligence. Creative Computing, 6, 44-53.

5. Putnam, H. (1960). Minds and machines.