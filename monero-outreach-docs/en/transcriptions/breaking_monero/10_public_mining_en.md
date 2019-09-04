# Breaking Monero 10: Public Mining Pools 
*06/09/19*  
_**Public mining pools provide a service to miners, but they also reveal a lot of output information that observers can use to learn more about transactions.**_  

**Breaking Monero Episode 10: Public Mining Pools**  

https://youtu.be/39HSRvuRgAo  
Public mining pools provide a service to miners, but they also reveal a lot of output information that observers can use to learn more about transactions. Sarang and Justin discuss the impact of these and ways users can optionally reduce their exposure to these risks. 

[All Breaking Monero Episodes](https://www.monerooutreach.org/breaking-monero/) 

_**Episode Transcription**_ 

_**Justin:**_ Hello and welcome back to another episode of Breaking Monero. Today we’re talking about public mining pool data. Public mining pools allow people to mine with the help of other people who share a block reward, but in doing so they often reveal a lot of information about the blocks that they mine and the transactions that they make. This transparency is good for miners, miners want to know when pools find blocks and when they send out payments, of course. They want to check to make sure that the pool isn’t trying to rip them off in some sort of fashion, but this is generally bad from the rest of the network’s perspective because the pool is giving a high level of visibility to the outputs that it handles which ultimately could impact the privacy of people on the network. Individuals can use the information from the blocks that they mine and the transactions that they send in order to compile a list of the outputs that the pool controls, and just like any other large output list that someone publishes, it could cause some damage. In the case of large public pools, this actually becomes a large enough proportion of the total Monero outputs that we need to start to pay attention. Sarang, can you talk a little more about what the public pools make public and why this is important? 

_**Sarang:**_ It really depends on what the pool is doing, so of course when any miner, whether they are part of a pool or mining solo, it doesn’t really matter, the network doesn’t care. The block contains a special transaction called the Coinbase Transaction which generates the reward for the miner or the operator of the mining pool, whoever is collecting for that group. It includes an output that is specially designated and specially identifiable as being for the reward for the miner. If you’re a solo miner, if I’m just mining on my own computer, I think “hooray,” I get to keep it and then later I can spend that in a transaction or do whatever I want with it. So there aren’t a whole lot of issues with that. So if your a solo miner and your just spending stuff as normal, then everything is pretty good. But if you’re part of a mining pool, the block reward goes to one address because that’s how the network is setup, and then that pool needs to send out shares of the mining reward to the participating miners according to whatever algorithm they use to determine that. And that typically looks like a payout transaction. So a payout transaction will often look like, one or maybe a couple more inputs going into the transaction to be spent and then a whole bunch of outputs going going off to destinations that the network can’t identify of course, these are one-time addresses, but we might be able to infer that they are destined for the individual miners who are participating in the pool. So when we occasionally see these transactions, which have one or two inputs and whole bunch of outputs we can probably identify them as miner payouts. Again, most transactions, if I’m just making a purchase or something may have a few different inputs, but typically will have two outputs. The destination for whatever I’m spending the money on and the change that will be directed back to me. So these definitely stand out on the network, and the way that Monero’s transaction protocol is setup, while ideally we can’t tell which inputs are being spent and where the outputs are destined for, we can identify how many of them there are. We can definitely see how many inputs are being spent and how many outputs are being generated. And of course a mining pool, in the interest of transparency might also publish lists identifying which Coinbase outputs are being generated for rewards for that pool and which transactions contain payouts. So there is data that is either on the chain already, and a clever or not so clever adversary might try to glean some information from and the pools might publish all or some of that information on their end for transparency too. 

_**Justin:**_ Excellent, thanks Sarang. To go over some of the goals that we have, from a Monero network perspective. Number one, we want to make sure that Monero’s ring size can account for these known outputs, these heuristically dead outputs. That involves making an analysis to see what the ultimate impact is, the proportion of what these outputs are and making sure that the ring size can meet these requirements because even if we go through a list of other mitigations, there can always be bad actors. We might make a list of best practices to follow and they might just ignore them for the purpose of malicious behavior or any other actor could try to do malicious behavior. So we need to make sure Monero’s ring size meets these requirements. Also, we want to keep the requirements on pools light and maintain as much transparency as possible. We know that from a miner’s perspective, they want to know when blocks are being mined in order to keep pools accountable. So the ideal solution would be one that allows pools to share as much information as possible. Also, keeping the impact to the network as light as possible. We could recommend that everyone churns all the time, but that’s not necessarily good for the networks health if we recommend that everytime a pool finds a block, they create six transactions. That’s not something that would be good for everyone overall. And then we also want to avoid wallet interaction, if wallets need to be scraping API data and other sorts of information it gets burdensome for the user and it’s prone to being fed incorrect or malicious information. Ultimately, the main goal is to preserve the integrity of the outputs. So pools can send transactions, they can publish information, but we want to make that in doing so we don’t mark the outputs that they control as known spent within these time periods. Ideally we want to make so that if these outputs are included in other ring signatures that we can’t, as an outside observer, immediately exclude these as known bad. 

_**Sarang:**_ I think it’s also worth noting that with all this introduction stuff it makes it sound like “my goodness, are public pools bad for the network?” if there’s all these things that we can indentify and could go wrong. I’d say absolutely not, a lot of people do not mine on their own because depending on the computational resources you have, you’ll have a lot of variability in when you’re going to get payouts, and that’s not great for the individual miner. So public pools reduce the variability for individual miners and in some sense can encourage someone to mine who might not have otherwise mined. There’s definitly a benefit to getting more miners on the network in this organized way, but we do have to be careful that whatever behavior we’re choosing, that it’s easy to do the right thing. And that pools that don’t do the right thing, for economic or incentive based reasons aren’t harming the network. 

_**Justin:**_ Thanks Sarang, so I’m currently sharing my screen regarding some of the public mining pool data, going over my Defcon slides just so you can see in a little more detail. We can divide the pool outputs into two main categories. We have the Coinbase outputs, these are new outputs that they mine out of blocks and then they have what I’m calling payment outputs. They are outputs that individuals send in transactions and so we have to account for them differently. There’s a number of strategies that we can use to approach them. I’ve listed them under their respective categories. There isn’t really a great option for these Coinbase outputs, we can say that pool’s should not publish a list, but of course that has it’s own set of downsides. We could say that they should churn secretly, but that has it’s own bloat downsides. Ultimately the main users options are to mark the Coinbase outputs so they interact with the pool API to ask the pool what blocks have they mined and then they do not use those outputs, or they could just avoid the Coinbase outputs. There’s certainly some debate within the research community and Monero about what the best process is there. We’re not going to focus too much on these Coinbase outputs, but it’s something you should be aware of. Regarding the payments, this is something where pools have an option to make much less of an impact on the network and I’m going to walk you through how they can do this. Of course, they can again, not publish, but miners want to have this information public. They could also mark payment outputs so a pool might just publish a list of all the outputs they control and people would need to not use those, but of course that requires interaction and has a high potential for pools to feed malicious information to users. We have this other solution called ‘Modified Input Selection Algorithm’ which I’m going to walk you through. And what this simply does is adjust the outputs that pools use to send transactions in a way that protects the integrity of the outputs they’re receiving from these payment outputs. Let’s look at these really quick. Let’s say that a pool makes a payout transaction, in this case they’re only sending funds to two users and one output that’s going back to themselves. It’s a change output that’s going back to the pool. Now the concern is that these two individuals receive their payout, but let’s say that the pool creates another transaction where the only output that they control is obviously spent. By that what I mean is these other block outputs are outputs that the pool does not control, so you know that they’re not spent. Since you have a list of all of the outputs that the pool controls, you know that this is the actual output that was spent by the pool. As a result, you can mark all these other decoys as fake for this transaction. You can say then that you know now that this output is spent in this specific transaction, so as a result if this output appears in any other transactions, I know it is fake. That could have a potential impact on the network if there are enough of those cases. However, instead, if the pool decides to select outputs in it’s ring from transactions that it sends to the users, so it makes a payout to these users and has it’s change output again and uses all three of these in a ring signature, sure you could still mark the black ones here as decoys because there’s no way for the pool that have actually spent those, but it theoretically could have spent any of these because you don’t know which of the outputs in this transaction was the change output. As a result, the change output here now looks the same as any other legitimate mining payout and preserves the integrity of this output. This output no longer needs to be marked by wallet clients. You can see, if we compare that to any other miner transaction it would look the same as a transaction that would just use the miner payout or change output as a decoy. So that’s some really basic information covering how public mining pool data, especially in regards to the payouts can go through a novel solution where there’s no additional harm to the network, the pools can still publish all the information that’s needed. They can just make a really small tweak in order to preserve the integrity of the payouts it makes. I thought that was pretty interesting when I had this revelation for these outputs. Sarang, can you talk a little bit more in depth about some of the analysis about how these outputs react to people? 

_**Sarang:**_ Yes. A lot of the different kinds of analysis that we’ve been talking about in previous episodes and maybe talking about in future episodes are about the idea of whether or not it’s known or suspected that a given output is spent or unspent. Obviously in assets like Bitcoin, you can identify unspent transaction outputs and spent transaction outputs, it’s built into how the transaction protocol works. But in Monero of course, ideally we don’t want to know if transaction outputs are unspent or spent because then we can identify them as decoys or not in particular transactions. So as Justin was mentioning, under certain circumstances you may be able to remove, heuristically in your head as you’re analyzing the different transactions, you may be able to internally flag or remove certain ring members as decoys or see if you can just flat out guess the true spent. Of course, these are heuristics and absent other information, there’s no way to tell for sure, but this exacerbates that. If we’re dealing with, for example, a chain split which is something we talked about before, which if done improperly can allow folks to flag outputs as decoys. Or if there were to be a chain reaction caused by something else, we know that that can also allow people to flag certain outputs as decoys or not. This kinda fits into this whole broad idea that we’ve been hinting at and talking about through this whole series, which is that anything that allows us to identify or statistically guess if something is a decoy or not is not good and can fit into these other forms of analysis. So our goal is to make everything look uniformly the same. This is of course very tough to do, a lot of times it requires us to put other sorts of burdens on the chain. For example we could require that all transactions have a certain fixed number of outputs even if they don’t need them, but that can cause a lot of bloat and work too which is a contentious thing to do. It is worth noting too, that unlike some of the other types of analysis, where you can look at an output, and proveably for sure know if it’s spent or not using set theory or graph analysis, the stuff that you’ve been talking about Justin are typically something that might be heuristically identifiable. Which means that it’s a guess, a statistical guess based on certain things you suspect about transaction patterns or behavior. Absent of information, nothing can be proven about that, but it could allow the adversary to make a pretty good guess. 

_**Justin:**_ Sarang, what are the key take aways for people who are watching this episode? Do people need to be concerned about these and if so, what can they really do to protect themselves? 

_**Sarang:**_ In general, like you were saying, the idea of having a ring size that has gotten larger over time is a kind of layered approach. So increasing ring size is one thing that can be done to mitigate, but certainly not eliminate certain forms of analysis we’ve been talking about, including this one. The idea is that, if for whatever reason, you choose your ring member decoys poorly and happen to be caught up in something like a public pool or a chain split or something, even if an adversary could flag that as the decoy, if your ring size is large enough and the effects of this analysis isn’t large enough, you still have the benefit of all the other decoys in your ring. In general, increasing ring size over time, which is something we do, other projects have different philosophies on the same ideas, is something that can be done to mitigate. And, of course, we have a spent output tool, formerly called ‘the blackball tool’ I don’t really like that word, but there is a tool that you can run that will download lists of spent outputs, at your own risk, or run it yourself to make sure that your wallet avoids selecting outputs that are either proveably dead or that are heuristically dead. Is this necessary for most people? I would argue, probably not. If it’s something that you’re really concerned about based on your security risk model, it’s something you could certainly consider. There’s resources on how to run that, how to do it, and how to use it safely. It’s also worth noting that this is a particularly tricky one, this falls into the question of “What do we do with Coinbase outputs and with things we suspect relate to pools?” There are some proposals that say maybe we only deal with Coinbase outputs with rings in certain ways. Or when we’re picking decoys, maybe we avoid Coinbase outputs or use them only in particular patterns. And these are tricky because they all come with different tradeoffs and different consequences and it’s definitely and area of ongoing research. On how we handle these both with default wallet behavior or on a more network wide required level. We don’t have a solution now that I think everyone is completely happy with, but it’s something that we’re really actively working on right now. 

_**Justin:**_ I have one final note too. The public mining pool data, over time, is a reliable source of information that attackers or observers can use in conjunction with other information that they’re trying to find. If an attacker is trying to do a chain split attack, and let’s say they only get 50% of the outputs over a certain time period to be observable from this chain split attack. But 20% of the outputs from that time period were observable as a result of the pools. A smart observer would be looking at the 50% and then the extra 20% to be able to do a test of about 70% of the outputs. The reason why this pool information is so important is it’s a recurring sort of information that could help contribute to other forms of attack and make them more powerful because it’s a contant, reliable source of information that people can use. 

_**Sarang:**_ Yes, that’s a really good point. The various chain splits that we might tend to worry about, there’s only very, very few of them that we’ve seen so far. But you’re right, mining pools operate all the time. Coinbase outputs are generated very reliably and as a result, mining payouts also happen very reliably. You might not have to worry very much about chain splits, and for the most part, we tend not to, but you’re right, there’s a lot of data that will continue coming in with public pool outputs. We’re continuing to work really hard to allow folks to continue to choose the network without having to worry about these sorts of things. 

_**Justin:**_ All right, thanks. Any final comments Sarang? 

_**Sarang:**_ Active area of research. As with a lot of other forms of analysis, it’s something that’s very tricky, it’s something that we are aware of and acknowledge that it is a limitation of the way that Monero is structured. Part of it is just the ways that different users behavior can affect the behavior of others, and some if it is how we decide to design our wallets and our transaction protocols. Our goal is to continue to make them better. 

_**Justin:**_ All right. Thank you Sarang for joining us today and thank you, the viewer for watching another episode of Breaking Monero. Take care. 