# Ruin
<img src="https://github.com/user-attachments/assets/a245ecd0-28f1-41de-830a-f050be9585d5" alt="ruin" width="100%">

## Short Game Description  

A 3D puzzle/exploration game with horror elements.  
You travel through a temple with a goal to restore the godess who controls the land, but beware the godess has a vengence and is doing everything to stop you.  

## My contribution  

During the project I've designed the puzzles and how they should be structured.  
I also did all the music and sound effects and created a script to use them in a 3D enviorment.  
I was also in charge of creating how the UI functions.  

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
  
