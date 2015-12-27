# How to Error Detection and Correction

Data protection and checking takes place various places throughout a system. Some of it is in hardware and some of it is in software. The goal is to ensure that data is not corrupted (changed), either coming from or going to the hardware or in the software stack. One key technology is ECC memory (error-correcting code memory).

The standard ECC memory used in systems today can detect and correct what are called single-bit errors, and although it can detect double-bit errors, it cannot correct them. A simple flip of one bit in a byte can make a drastic difference in the value of the byte. For example a byte (8 bits) with a value of 156 (10011100) that is read from a file on disk suddenly acquires a value of 220 if the second bit from the left is flipped from a 0 to a 1 (11011100) for some reason.

ECC memory can detect the problem and correct it so with the user unaware. Notice, however, that only one bit in the byte has been changed and then corrected. If two bits change – perhaps by both the second and seventh from the left – the byte is now 11011110 (i.e., 222); typical ECC memory can detect that the “double-bit” error occurred, but it cannot correct it. In fact, when a double-bit error happens, memory should cause what is called a “machine check exception” (mce), which should cause the system to crash. After all, you are using ECC memory, so ensuring the data is correct is important; if an uncorrectable memory error occurs, you would probably want the system to stop.

The source of bit-flipping usually originates in some sort of electrical or magnetic interference inside the system. This interference can cause a bit to flip at seemingly random times, depending on the circumstances. According to the Wikipedia article and a paper on single-event upsets in RAM, most single-bit flips are the result of background radiation – primarily neutrons from cosmic rays.

The same Wikipedia article reports that the error rates reported from 2007 to 2009 varied all over the map, ranging from 10–10 (errors/bit-hr) to 10–17 (seven orders of magnitude difference). The lower number is just about one error per gigabit of memory per hour. The upper number indicates roughly one error every 1,000 years per gigabit of memory.

A study of real memory errors took place at Google. During their investigations they found that one third of the machines and more than 8 percent of the DIMMs saw correctable errors per year. This translates to Google experiencing about 25,000–75,000 correctable errors (CE) per billion device hours per megabit, which translates to 2,000–6,000 CE/GB-yr (or about 250–750 CE/Gb-yr). This is much higher than the previously reported “high” correctable error rate of 1 CE/Gb-yr (250–750 times higher) and six orders of magnitude higher than the optimistic report.

The study went on to report some other interesting results that bear repeating here.

* Memory Errors are strongly correlated
 * There is a strong correlation among correctable errors within the same DIMM. A DIMM that has a correctable error is 13–228 times more likely to see another in the same month.
 * An uncorrectable error is preceded by a correctable error 70–80 percent of the time. A correctable error increases the probability of an uncorrectable error by factors of 9–400.
 * Uncorrectable errors following a correctable error are still small at 0.1%–2.3% per year.
* The incidence of correctable errors increases with age, but the incidence of uncorrectable errors decreases with age
 * The increasing incidence of correctable errors sets in after about 10–18 months.
 * The most likely reason for uncorrectable errors decreasing is that DIMMs with a large number of correctable errors are replaced, decreasing the likelihood of uncorrectable errors.
* There is no evidence that newer generationDIMMs have worse behavior (this study was published in 2009)
* Temperature had a surprisingly low effect on memory errors (over the temperature range tested)
* Error rates are strongly correlated with utilization
* Error rates are unlikely to be dominated by software errors

To me, these observations indicate that in real-world production, we see much higher error rates than what manufacturers are reporting. Moreover, the rate of correctable errors can be an important factor in watching for memory failure. Consequently, I think monitoring and capturing the correctable error information is very important.