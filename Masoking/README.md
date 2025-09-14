# Masoking
<img width="100%" height="600" alt="MasokingHeader" src="https://github.com/user-attachments/assets/56301df2-755e-4e44-bdb3-bace62480096" />

## Short Game Description  

You play as Masoking. A excentric masocist king who loves pain and plessure. 
He is ordering his jesters to cause him as much pain as possible, however the jesters don't really want to but does as he commands.
So the goal is to take as much damage as possible by the projectiles that are thrown by the jesters, but beware of your heatmeter if it goes low there is a chance that the king will become unsatisfied!

## My Roles in the project

During my time in the project I've helped with the movement, some off the jesters you encounter and also designed the music and soundeffects.
I have implemented a system to play sound effects and music, and I have also helped with the functionality off the menu.
The intro level is also designed by me, and I have also helped my fellow programmeres with different coding .

 <details>
  <summary>Details</summary>
   
Developed:  11/2024 - 01/2025  
Duration:   8 weeks  
Engine:     Unity  
Genre:      Bullet Hell 
Team:       3 Programmers, 4 Artist

  </details>

<table>
  <tr>           
   <td><img <img width="1920" alt="MasokingPic1" src="https://github.com/user-attachments/assets/16002089-c860-4421-a119-8fc016bedd04" />
   <td><img width="1920" alt="MasokingPic2" src="https://github.com/user-attachments/assets/8ddbcb0a-48bb-47be-8bb8-415d7aa972bc" />
 </td>
  </tr>
</table>

---

## Game objects I've designed

### Falling Jester

I've designed the green jester that falls when you dash into it. How they work is that they move one way to another and you are supposed to dash into it to make it fall over and then stand in the red line to take damage. It wobles with code and have some animation to seem like its tipping over

 ![Masoking Green Jester](https://github.com/user-attachments/assets/489ae74b-4bb1-4900-83dc-db4b66f2d158)


### Bomb

The bomb works by having a timer until it sets off and the you need to be near it to take damage, and when you take damage you also launch away by the explosion impact

![Masoking Bomb](https://github.com/user-attachments/assets/eb1ba741-0af7-4262-9f55-b038d2073625)


## Sound and Music Manager

  ### How it works

It is set upp with different functions to play sounds, with options to play loops or play single sounds, you assign the sounds you want to play on the objects that you want to play sounds, and call the script in other codes where you want it to play, or stop if you use loops. some sound effects also change pitch based on how often they are played.

<table>
<tr>

 <td> <img width="1142" height="1601" alt="Unity sound 1" src="https://github.com/user-attachments/assets/d5129281-42e5-4425-8b02-7a091f60f324" /> </td>
 <td><img width="1154" height="1601" alt="Masoking Sound" src="https://github.com/user-attachments/assets/5a8d7ecc-f26e-49c9-bf17-c3b06ce72f18" /></td>
 <td><img width="1155" height="1599" alt="Masoking sound 2" src="https://github.com/user-attachments/assets/21176e06-57a0-42e6-a9ab-c09f5308d6c9" /></td>
</tr>

</table> 

<details>
  <summary>How to play sounds</summary>

 ``` 
 public void PlaySoundFX(AudioClip audioClip, float volume)
{



    if (timer >= minTime)
    {
        soundFXObject.PlayOneShot(audioClip, volume);
        timer = 0f; // Reset timer after sound is played
    }

}


public void PlayRandomSoundFX(AudioClip[] audioClip, float volume)
{
    int rand = Random.Range(0, audioClip.Length);
    PlaySoundFX(audioClip[rand], volume);
}
  ``` 

</details>

<details>
  <summary>How to play and stop different loops</summary>

```
 public void PlayOnLoop()
 {

     Claps.clip = claps;
     Claps.loop = true;
     Claps.volume = 0.6F;
     Claps.Play();
 }

 public void StopLoop()
 {
   
     Claps.loop = false;
 }


 public void StartWalking()
 {
     if (!isWalking && walkFX != null && walkClip != null)
     {
         isWalking = true;
         walkFX.clip = walkClip;
         walkFX.loop = true; // Optional: Set to `true` for continuous playback
         walkFX.Play();
     }
 }

 public void StopWalking()
 {
     if (isWalking && walkFX.isPlaying)
     {
         walkFX.Stop();
     }
     isWalking = false;
     walkFX.loop = false;
 }
``` 
</details>


  
