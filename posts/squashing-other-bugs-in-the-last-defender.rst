.. title: Squashing other bugs in The Last Defender
.. slug: squashing-other-bugs-in-the-last-defender
.. date: 2017-04-15 15:26:54 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

Previously I had only talked about the memory bugs in our game. This post is about the other kind of bugs that we had squashed. The most common issue that we faced in the past few weeks(because we were rushing for submission) were XML issues, untested APIs and lots of logic issue. Our engine only had a simple serialization/deserialization/property system and so this led to multiple issues. Of which the common ones that we faced were because of the file merging issue. When we merge our level files, the merge sometimes didn't merge properly which causes our xml file to be out of date than the other person's. The solution that we came up with to circumvent that was to have our scene objects load from our prefab file if a prefab was specified. That way we can modify the prefab and not affect the scene at all.

Other bugs that we encountered is untested API which is very common for us because when we were writing the components, usually we are the ones that will use the API. This in turn causes our API to not be thoroughly tested as it is. A few example of bugs like this was the changing scene and sound API bug. The former is when the engine invokes a scene change, it will crash and the latter involving sound was not heavily used and tested therefore a few bugs in the sound system.

Aside from that, we also have bugs with our Math quaternion library and rotations. Initially we do not really understand about quaternions but as we incorporated it, we realized that rotations is not that simple and we have to see what is the order of the rotation and every method is different from each other. With that I will conclude this posts. Hopefully we can get to work on other new stuff for the upcoming GAM 350.

