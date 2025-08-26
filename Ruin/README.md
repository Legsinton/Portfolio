# Ruin
<img src="https://github.com/user-attachments/assets/a245ecd0-28f1-41de-830a-f050be9585d5" alt="ruin" width="100%">

## Short Game Description  

A 3D puzzle/exploration game with horror elements.  
You travel through a temple with a goal to restore the godess who controls the land, but beware the godess has a vengence and is doing everything to stop you.  
How you restore her is by collecting different body parts off the statue which are locked behind gates and doors, and you need to solve different puzzles to get to them. Theres exploration so you can pick up different items to get clues and lore to find out what has happened to the goddess.

## My Roles in the project

During the project I've designed the puzzles and how they should be structured and with the help off my coworkers to make them feel as good to control as possible.  
I also did all the music and sound effects and created a script to use them in a 3D enviorment.  
I was also in charge of creating how to use the menu and title screen.
And also co created, movement and how to interact with everything.

 <details>
  <summary>Details</summary>
   
Developed:  04/2025 - 06/2025  
Duration:   8 weeks  
Engine:     Unity  
Genre:      Puzzle, Exploration, horror  
Team:       3 Programmers, 4 Artist

  </details>

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/e8fef112-98a1-4d10-98b8-e6355a14add0" width="1244" alt="RuinPic1" /></td>
    <td><img src="https://github.com/user-attachments/assets/9c2290d4-a89b-428e-ba10-5bdf257331c7" width="1244" alt="RuinPic2" /></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/ef16b1eb-100d-4e50-885b-06519d8fe08f" width="1244" alt="RuinPic3" /></td>
    <td><img src="https://github.com/user-attachments/assets/f602bb20-b5f4-40d2-9d2b-d43dbde76808" width="1244" alt="RuinPic4" /></td>
  </tr>
</table>  

---

### Puzzles I've designed

<details>
  <summary>Info</summary>

### Consepts

First idea was a block puzzle in which you move blocks on platforms to make things move or open, so the first one is to make the platform move down to get the other block to the other floor, and then use one off the triggers to get another block to push into the other trigger to open the gate on the left side.  

Second idea was a clock puzzle which you controll a standing turner that has a minute and hour trigger that you need to put on the right "time" the time you get by exploring.  


 

<table>
 <tr>
  <td>![RuinPuzzleRoom1Portfolio](https://github.com/user-attachments/assets/3df54984-69cb-48d6-8565-0ba4bb756252)</td>
 </tr>
 
</table>

</details>


<details>
  <summary>SoundFX Script</summary>

 ``` 
   public void PlaySoundFX(SoundType type, Vector3? position = null,float minDistance = 1f, float maxDistance = 50f, float spatialBlend = 1f)
 {
     if (!soundFXDict.ContainsKey(type)) return;

     // volume if its not in the inspector
     float volume = 1.0f;
     if (soundVolumeDict != null && soundVolumeDict.ContainsKey(type))
     {
         volume = soundVolumeDict[type];
     }

     AudioClip clip = null;
     if (soundFXDict[type] is AudioClip singleClip)
     {   // If single soundFX
         clip = singleClip;
     }
     else if (soundFXDict[type] is AudioClip[] clipArray)
     {   // If Multible soundFX
         clip = clipArray[Random.Range(0, clipArray.Length)];
     }

     if (clip == null) return;

     if (position.HasValue)
     {
         // Manual PlayClipAtPoint with custom size
         GameObject tempGO = new GameObject($"SFX_{type}");
         tempGO.transform.position = position.Value;

         AudioSource aSource = tempGO.AddComponent<AudioSource>();
         aSource.outputAudioMixerGroup = sfxMixerGroup;
         aSource.clip = clip;
         aSource.spatialBlend = spatialBlend;
         aSource.minDistance = minDistance;
         aSource.maxDistance = maxDistance;
         aSource.volume = volume;
         aSource.Play();

         Object.Destroy(tempGO, clip.length);
     }
     else
     {
         soundFXObject.PlayOneShot(clip, volume);
     }
 }
  ``` 

</details>

<details>
  <summary>SoundFX Script</summary>

```
private void InitializeSounds()
{   // To find and add soundFX in assets
    soundFXDict = new Dictionary<SoundType, object>
        {
            //Single AudioClips
            { SoundType.ButtonSelect, Resources.Load<AudioClip>("Sounds/Effects/ButtonSelect") },
            { SoundType.Boom, Resources.Load<AudioClip>("Sounds/Effects/Boom") },
            { SoundType.Chain, Resources.Load<AudioClip>("Sounds/Effects/Chain") },
            { SoundType.ChestCreak, Resources.Load<AudioClip>("Sounds/Effects/ChestCreak") },
            { SoundType.ChestOpen, Resources.Load<AudioClip>("Sounds/Effects/ChestOpen") },
            { SoundType.Death, Resources.Load<AudioClip>("Sounds/Effects/Death") },
            { SoundType.Fire, Resources.Load<AudioClip>("Sounds/Effects/Fire") },
            { SoundType.KeyFound, Resources.Load<AudioClip>("Sounds/Effects/KeyFound") },
            { SoundType.ManScream, Resources.Load<AudioClip>("Sounds/Effects/ManScream") },
            { SoundType.PushBlock, Resources.Load<AudioClip>("Sounds/Effects/PushBlock") },
            { SoundType.PuzzleSolved, Resources.Load<AudioClip>("Sounds/Effects/PuzzleSolved") },
            { SoundType.PuzzleSolvedFully, Resources.Load<AudioClip>("Sounds/Effects/PuzzleSolvedFully") },
            { SoundType.DoorOpen, Resources.Load<AudioClip>("Sounds/Effects/DoorOpen") },
            { SoundType.PressurePlate,Resources.Load<AudioClip>("Sounds/Effects/PressurePlate") },
            
            
            //Multible AudioClips
            { SoundType.ButtonSound, Resources.LoadAll<AudioClip>("Sounds/Effects/ButtonSound") },
            { SoundType.Break,Resources.LoadAll<AudioClip>("Sounds/Effects/Break") },
            { SoundType.Goddess, Resources.LoadAll<AudioClip>("Sounds/Effects/GoddesScream") },
            { SoundType.GoddessAngry, Resources.LoadAll<AudioClip>("Sounds/Effects/GoddesScreamAngry") },
            { SoundType.Insert,Resources.LoadAll<AudioClip>("Sounds/Effects/Insert") },
            { SoundType.RandomScary,Resources.LoadAll<AudioClip>("Sounds/Effects/RandomScary") },
            { SoundType.Walk, Resources.LoadAll<AudioClip>("Sounds/Effects/Walk") },
            { SoundType.WalkSoft, Resources.LoadAll<AudioClip>("Sounds/Effects/WalkSoft") },
        };
}
```

</details>
  
