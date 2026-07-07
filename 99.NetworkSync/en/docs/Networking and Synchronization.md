<br>

# 📝 MapleStory Worlds Networking and Synchronization

<br>

## Learning Goals

- Understand the server-client model.
- Understand None and Sync.
- Understand Server, ServerOnly, Client, ClientOnly, and Multicast.

<br>

## Server-Client Model Theory

(Reference: https://maplestoryworlds-creators.nexon.com/ko/docs?postId=207)

<p align="center">
  <img src="../../_images/1778638939193_CommonTraining_02.png" alt="메월드타이틀이미지" />
</p>

In this lecture, we will go over the synchronization with MapleStory World's networking. First let's understand server-client theory and then look into the various network settings that are provided by MapleStory Worlds. 


<p align="center">
  <img src="../../_images/1637228699029a52751daf6c943c0a46ca3ca575c1f9e.png" alt="서버클라이언트모델" />
</p>

- If a MapleStory account is created from a single PC, that account can still be used to play the game on a laptop or in a PC cafe environment. This is possible because the information is loaded from a 'single destination' no matter what device you log in from.

 - The 'single central space' where the data is safely saved to and sent from is the server.

 - Each 'individual device' that shows the screen by logging into that server and allows users to make inputs is the client.

- The games on MapleStory Worlds also function in the same way.

<br>

---

<p align="center">
  <img src="../../_images/1781136080829_image.png" alt="서버클라 도식화2" />
</p>

- The most widely-used standardized structure for network games is this 'server-client model'. 
- The client is an area the player encounters directly for gameplay. 
- Play will be referred to as the User. It serves the role of taking inputs from the User from the keyboard or mouse, delivering them to the server, and then taking the information from the server and building it on the monitor to see.
- The server is in charge of processing numerous requests and then delivering that information to all users in real time at the same time so that they can all see the same game state.

- The clients aren't connected to each other.

<br>

---

<p align="center">
  <img src="../../_images/1781136160657_image.png" alt="서버클라 도식화3" />
</p>

- This method is used because of the risk of data tampering--for security reasons.
- If the client processes important information and then delivers it to other people without going through the server, then there is a risk of the entire game collapsing when someone hacks into the game program through the computer.

<br>

---
<p align="center">
  <img src="../../_images/1781136315194_image.png" alt="서버클라 도식화4" />
</p>

- To remove the above risk, the client takes the user's input and delivers it to the server, the server processes important logic and calculates the results, the results are delivered to the client, and then the client takes the information received and outputs it to the screen.

- The server-client model leaves all permissions and data verification in the single absolute standard known as the 'server' to protect against hackers and maintain a fair gaming environment.

- The same can be said for MapleStory Worlds. When the main task is handled by the server, the result is reflected to the client.

- Assuming that synchronization is taking place, when the HP is decreased from battle, item use, etc. from the server, the changed HP will also be applied to the clients.


<br>

---

## Sync and None

- Sync means that a specific property will be synchronized, while None says that will not happen.

- Let's follow the project and learn more details about the difference between Sync and None.

<p align="center">
  <img src="../../_images/1780731911422_image.png" alt="월드 새로 생성" />
</p>

- Create a new world.

<br>

<p align="center">
  <img src="../../_images/1780732185675_image.png" alt="스크립트 생성" />
</p>

 - Then, create a component script that will be attached to the Default Player.

 <br>

<p align="center">
  <img src="../../_images/1780732417695_image.png" alt="스크립트 프로퍼티" />
</p>

```lua
[None]
number MasterVolume = 10

[Sync]
number HP = 10

```
- Create 2 properties, one as None and one as Sync for testing.

<br>


<p align="center">
  <img src="../../_images/1780732568165_image.png" alt="하이어라키 패널 띄우는 법" />
</p>

- Press [Panels -> ServerHierarchy] to open the server hierarchy.

- This panel lets you view the entities and various information that exist on the server.

- Open the panel and press the ▶ play button at the top.

<br>

<p align="center">
  <img src="../../_images/1780732715480_image.png" alt="서버 엔티티 수치 변경 후 엔터" />
</p>

- Press the player entity in the server hierarchy, change the stat from (10,10) to (5,5) and press Enter.

<br>



<p align="center">
  <img src="../../_images/1780732810108_image.png" alt="" />
</p>

- Looking at the client entity, we can see MasterVolume is 10 and HP is 5.

- The property with a Sync applied is synchronized with the server, and the property with None is not.

<br>


<p align="center">
  <img src="../../_images/1780733106331_image.png" alt="" />
</p>


<p align="center">
  <img src="../../_images/1780733150692_image.png" alt="" />
</p>



- Even if the client entity's stats are changed to (100,100), it will not be applied to the server.

- In the MapleStory Worlds official document, it says that when a property is set to Sync, the synchronization happens in a single direction from server to client.

- None has nothing to do with the server client direction, since it isn't synchronized.

<br>

---

## Server Only, Client Only

- Next, we'll take a look at Server Only and Client Only through a method.

- The Server Only method is only performed on the server entity, while Client Only is only performed in the client entity.

<p align="center">
  <img src="../../_images/1781137066575_image.png" alt="" />
</p>

```lua
[server only]
void OnBeginPlay()
{
    log("Executed at Server (Executed in Server)")
    self.HP +=10
    self.MasterVolume +=10
}
```

<br>

<p align="center">

  <img src="../../_images/1781137999003_image.png" alt="" />

</p>

- When reading logs, you need to confirm whether it says Server or Client on the front.

- Since the content in the in the server hierarchy has been performed and the stat was changed from the server, the client will change accordingly through sync.

<br>

<p align="center">
  <img src="../../_images/1781138965639_image.png" alt="" />
</p>

- Press Show More next to the method to view an option that can change the execution space.

- Press Execution Space Settings.

<br>

<p align="center">
  <img src="../../_images/1781139126778_image.png" alt="" />
</p>


<p align="center">
  <img src="../../_images/1781139274484_image.png" alt="" />
</p>

- Change the function execution space on top of OnBeginPlay() to client only. Also change the annotation to "executed from client".

- Now, when played, the server hierarchy response no longer exists, while the client hierarchy left a log.

- Since the stat changed in the client, it is not reflected in the server.


<br>

<p align="center">
  <img src="../../_images/1781139454211_image.png" alt="" />
</p>


<br>

<p align="center">
  <img src="../../_images/1781139502671_image.png" alt="" />
</p>

- When changed to Not Used, the server hierarchy entity will be performed in the server while the client entity will be performed in the client.

- When looking at the logs, the code was executed in the server first and then in the client.

- Let's predict what the HP and MasterVolume stats for the server entity and client entity will be before clicking on them to verify.

<br>

<p align="center">
  <img src="../../_images/1780880700228_image.png" alt="" />
</p>

- Pressing the server hierarchy's entity will show that the volume and HP are set to 20,20. 
  - It's understandable that it would be (20,20) in the server, but the client may be confused whether it's (20, 20) or (20,30).
  - It's possible that the stats are all reflected from the server, followed by the client performance.
  - Or it's possible that the server and client both quickly reflected the stats, and since there was no difference, a synchronization wasn't necessary.

- There is a way to check the order of operations through code for complicated situations like these.

- OnSyncProperty is a method that loads automatically when a property synchronization happens, and you use this to see when the Sync happened and the synchronized stats.


<br>

<p align="center">
  <img src="../../_images/1781142830225_image.png" alt="" />
</p>

https://maplestoryworlds-creators.nexon.com/en/docs/?postId=207

- We can see from the MapleStory Worlds document that Sync has a delay.

- To know when and where the Sync happens, let's learn about the **IsClient() method, IsServer() code**, and **OnSyncProperty**.

<br>

<p align="center">
  <img src="../../_images/1781143206306_image.png" alt="" />
</p>

- Through if statements, a code that waits for 2 seconds if something is being performed in the client was written. 

- IsClient() returns True if it's being performed in the client while IsServer() returns True if it's being performed in the server.

- Let's add an annotation for confirmation.

<br>


<p align="center">
  <img src="../../_images/1781143396688_image.png" alt="" />
</p>

- OnSyncProperty is a method that's called when property synchronization occurs, or when Sync is performed. The name of the property being synchronized goes to name, while the stat being synchronized goes into value.

- If the name is the same as HP, in other words if the property being synchronized is HP, write a code that looks at value and proceed with testing.

<br>

<p align="center">
  <img src="../../_images/1780880783353_image.png" alt="" />
</p>

- If the client side code is performed 2 seconds later, we can tell the order of synchronization.

- We can see that the server reflects the stat, the sync happens in the direction of server to client, and then the stats are reflected onto the client.


<br>

<p align="center">
  <img src="../../_images/1781144809415_image.png" alt="" />
</p>

<p align="center">
  <img src="../../_images/1781144851734_image.png" alt="" />
</p>

- Remove the code that tells the client to wait (or leave an annotation) and run the test again.

- Seeing as the OnSyncProperty exists in the script but not in the log, we can tell that the Sync didn't happen.


<br>

<p align="center">
  <img src="../../_images/1780880834991_image.png" alt="" />
</p>

- In summary, IsClient() returns true only when operating within the client while IsServer() returns true only when operating within the server.

- Through this we can create conditional statements based on usage area like in the screenshot above, but the code is disorganized and the upkeep looks bad.


<br>

<p align="center">
  <img src="../../_images/1781145511358_image.png" alt="" />
</p>

- By making separate methods like this, by properly linking client only and server only, the method in the proper performance space will activate automatically.

- Making additional methods and separating the performance areas is something that you will see often when navigating remake worlds.

<br>

---

## Server, Client, Multicast



<p align="center">
  <img src="../../_images/1780880878453_image.png" alt="" />
</p>

- Usually the UI only needs to be visible in the client. Let's make a model for UI output.

- Also attach UITransform and TextComponent to the created model. Adjust the size and position, so it's easily viewable.


<br>

<p align="center">
  <img src="../../_images/1780880899211_image.png" alt="" />
</p>


- Set the UI that was just created as the child of Default Player through SpawnService. 

- Set the play space for OnBeginPlay to server only.

<br>

<p align="center">
  <img src="../../_images/1780880913938_image.png" alt="" />
</p>

- Adjust the detailed position through play.


<br>

<p align="center">
  <img src="../../_images/1780880928059_image.png" alt="" />
</p>

- We can see that the spawnService that's performed in server only is created in the server, which is then reflected to the client.

- We can also see that the HP UI entity is created in the server hierarchy and client hierarchy.


<br>

<p align="center">
  <img src="../../_images/1780880959335_image.png" alt="" />
</p>


<br>

<p align="center">
  <img src="../../_images/1780880978327_image.png" alt="" />
</p>


- Now change the execution space to client only.

- Write a code so that the HP property's stats can be visible from the text component that was just made.

- 2 things can be seen when playing.
  - First, the HP UI entity is only created in the client. (This is because the execution space is set to client only.)
  - Second, the property stat is reflected properly to the client's UI entity.


<br>

<p align="center">
  <img src="../../_images/1780880995335_image.png" alt="" />
</p>

- When testing OnBeginPlay() after reverting it back to server only, Text will be visible on the screen instead of 10.

- 10 is reflected on the server hierarchy's HP UI entity but not the client's HP because there is no code that tells the client's HP UI to update.

- Now, let's learn how to make commands to the client when performing in the [server] space.

<br>


<p align="center">
  <img src="../../_images/1780881009536_image.png" alt="" />
</p>

- Make a new method but change the execution space to client.

- OnBeginPlay is only performed in the server, since it's set to server only, but when calling a method that is set to client, it will be performed in the client.

<br>

<p align="center">
  <img src="../../_images/1780881022610_image.png" alt="" />
</p>

- We can see that the situation was reversed. Text has been entered into the HP UI entity and 10 has been entered into the client.

- Now let's learn about Multicast, which executes methods for both the server and client.


<br>

<p align="center">
  <img src="../../_images/1780881035983_image.png" alt="" />
</p>

- Placing the cursor over Multicast, we can see that 'when calling a method that can only be called from the server, everything is executed in the Server Client.'

<br>

<p align="center">
  <img src="../../_images/1780881048119_image.png" alt="" />
</p>

- After changing from client to multicast, we can now see that both UI entities have been changed.
  - Use the Client if you want the client to perform methods even if it's performed in the server.
  - We have confirmed that Multicast is used to allow performance on both sides.

- Finally, we have the server-side execution that is triggered from the client space, which will now use key inputs to demonstrate this.

<br>

<p align="center">
  <img src="../../_images/1780881068522_image.png" alt="" />
</p>

- When making a key-down event, we can see that it's performed in the Client through annotations.

- Write a code for a reduction by 1 when Q is pressed.




<br>

<p align="center">
  <img src="../../_images/1780881083329_image.png" alt="" />
</p>

- Looking at the client hierarchy, we can see that the HP is dropping each time Q is pressed.

- Even if the HP is decreased from the client, due to the properties of the server client model, the decreased HP won't be synchronized to the server.

- Now we will make it so that important logic like HP changes operate in the server.



<br>

<p align="center">
  <img src="../../_images/1780881098944_image.png" alt="" />
</p>

- According to the server content, when calling from the server or client, performance will be in the server. 

- Write a logic that changes HP in the server. Then set the execution space to server.

- Now call the method so that it can take place in the server without changing the HP stat when a key input happens.


<br>

<p align="center">
  <img src="../../_images/1780881113514_image.png" alt="" />
</p>

- Now when a key input happens, the HP change happens in the server, and this stat is also reflected in the client via synchronization.


<br>

<p align="center">
  <img src="../../_images/1780881360843_image.png" alt="" />
</p>

- We learned that the UI doesn't need to exist in the server. Its existence in the client means that the users can see it, so the Spawn only happens in the client.

- Let's make a logic that updates the UI when the HP changes from the server through OnSyncProperty.

- We completed the logic that updates that when Q is pressed, the HP is changed in the server as well, and the UI is updated accordingly.

<br>

---

## Closing Remarks

kor: https://maplestoryworlds-creators.nexon.com/ko/docs/?postId=210

eng: https://maplestoryworlds-creators.nexon.com/en/docs/?postId=210


<br>

<p align="center">
  <img src="../../_images/165640110289849c2637a9a3c4b6ea395b2e202164e89.png" alt="" />
</p>

<p align="center">
  <img src="../../_images/1781145963603_image.png" alt="" />
</p>

- We learned about the difference between Sync and None. 

- Stats and content that need to be synchronized use Sync.

- Individual setting values that don't need synchronization use None.


<p align="center">
  <img src="../../_images/165640128942696725f39866847439127557f4942f7c0.png" alt="" />
</p>

<p align="center">
  <img src="../../_images/1781146019102_image.png" alt="" />
</p>


- We confirmed that client, client only, server, server only, and multicast is used for execution space control.

- Opening and closing the UI, controlling the volume, etc. don't need to be synchronized to the server and thus can be performed in the client.

- Important calculations, character stat changes, etc. are performed in the server.

- Methods that are only performed in the server and methods that are only performed in the client can be seen in the above image.

- Knowing which execution space setting to use for the situation will allow for an easy programming experience when using the server-client model.