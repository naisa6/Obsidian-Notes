## Obfuscating attacks using encodings

Simply copying the attacks from our lab solutions and attempting them on real sites will only get you so far. Websites that you test will have often already been audited by other users and had a number of patches applied to them. To take your skills further, you'll need to adapt the techniques you've learned to overcome these additional obstacles, unearthing vulnerabilities that other testers may have overlooked.

In this section, we'll provide some suggestions on how you can obfuscate harmful payloads to evade input filters and other flawed defenses. Specifically, you'll learn how to use standard encodings to take advantage of misconfigurations and handling discrepancies between connected systems.

## Using Burp Scanner during manual testing

Testing for certain types of vulnerability can be fairly tedious, especially ones that involve trying numerous injection techniques in every controllable input. Doing this manually is often impractical due to real-life time constraints, which can lead to you missing critical vulnerabilities.

There are a number of ways you can optimize your workflow by using Burp Scanner to supplement your own knowledge and intuition. Not only does this reduce the chance of you overlooking things, it can save you valuable time by helping you to rapidly identify potential attack vectors. This means you can concentrate your time and effort on things that can't be easily automated, such as working out how to exploit the vulnerable behavior or chain it with your other findings.

## Identifying unknown vulnerabilities

When attempting one of our labs, you usually know the exact vulnerability you need to look for. This is obviously not the case when testing genuine websites. To help you bridge this gap, we've created a mystery lab feature that lets you practice identifying vulnerabilities without any prior knowledge of them.