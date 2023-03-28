## Description

>Flag format: picoCTF{Malwarename} 
>The first letter of the malware name should be capitalized and the rest lowercase. 
>Your friend just got hacked and has been asked to pay some bitcoins to `1Mz7153HMuxXTuR2R1t78mGSdzaAtNbBWX`. He doesn’t seem to understand what is going on and asks you for advice. Can you identify what malware he’s being a victim of?

-------------

In this challenge, we are given a bitcoin address, and information that this address is used to siphon money from an innocent friend's bitcoin wallet. Since blockchain transactions are publicly disclosed, we can view the activity associated with this wallet on a quick Google search.

www.bitcoinabuse.com is a website dedicated to tracking bitcoin addresses used in ransomeware and blackmailings, so if our address was used in other previous attacks, we can find reports for our given address. 

Searching this address we see several reports for multiple abuse types. Since we know our friend had been hacked, the abuse type is likely a ransomeware attack. The first report against this address is labeled ransomeware, and provides a link where we might learn more:

>More information here: https://blog.avira.com/petya-strikes-back/

The title of the article plainly tells us the malware at play is based on the infamous ransomeware Petya, but we can also follow the link to learn more about it.

We are told the answer to the flag is the malware involved, so we can submit the flag by inserting "Peta" into the picoCTF flag format.