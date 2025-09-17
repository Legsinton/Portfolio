
# Ruin
<img src="https://github.com/user-attachments/assets/a245ecd0-28f1-41de-830a-f050be9585d5" alt="ruin" width="700" hight="50">

 <details>
  <summary>Details</summary>
   
Developed:  04/2025 - 06/2025  
Duration:   8 weeks  
Engine:     Unity  
Genre:      Puzzle, Exploration, horror  
Team:       3 Programmers, 4 Artist

  </details>

## Short Game Description  

A 3D puzzle/exploration game with horror elements. 
You travel through a temple with a goal to restore the goddess who controls the land but beware the goddess has a vengeance and is doing everything to stop you. 
How you restore her is by collecting different body parts off the statue which are locked behind gates and doors, and you need to solve different puzzles to get to them. Theres's exploration so you can pick up different items to get clues and lore to find out what has happened to the goddess. 

## My Roles in the project

During the project I've designed the puzzles and how they should be structured and with the help off my coworkers to make them feel as good to control as possible. 
I also did all the music and sound effects and created a script to use them in a 3D environment. 
I was also in charge of creating how to use the menu and title screen.
And also, co-created movement and how to interact with everything. 

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

## Puzzles I've designed

### Consepts

First idea was a block puzzle in which you move blocks on platforms to make things move or open, so the first one is to make the platform move down to get the other block to the other floor, and then use one off the triggers to get another block to push into the other trigger to open the gate on the left side. 

Second idea was a clock puzzle which you control a standing turner that has a minute and hour trigger that you need to put on the right "time" the time you get by exploring. 

<table>
 <tr>
  <td><img src="https://github.com/user-attachments/assets/7ec60570-b6dd-4a36-a0c8-439909734f75" alt="RuinPuzzleRoom1Portfolio" width="250" /></td>
  <td><img src="https://github.com/user-attachments/assets/82f0878a-5918-4335-a662-9d23e5037e62"  alt="RuinPuzzleRoom2PortfolioNew" width="250"/></td> 
  <td><img src="https://github.com/user-attachments/assets/3f2b8353-668a-4b0b-b6b0-e0436154b394" alt="RuinPuzzleRoom2Part2PortfolioNew" width="250"/></td>

   </tr>
</table>  


## Sound and Music Manager

  ### How it works

 The whole structure is a sound effects manager that works in 3D and is set up with Enums and Dictionaries. 
You have control to change size of the object in game, volume, spatial blend and if it should be looping or not. 
And then just call it wherever you want a sound or sound loop to appear.

<table>
<tr>

 <td> <img width="300" alt="Unity_now43zD16O" src="https://github.com/user-attachments/assets/cbe7accc-a158-4b6f-9528-db20ce43f7cf" /> </td>
 <td><img width="300"  alt="Unity_6gJHSoNKrd" src="https://github.com/user-attachments/assets/089f8c75-3b96-468d-826c-a4b4d277cc10" /></td>
 <td><img width="300" height="500" alt="Unity_w3M1l0cbli" src="https://github.com/user-attachments/assets/5d147295-b76c-4d1b-a0bf-5e62b47bc1bd" /></td>
</tr>

</table> 

### Code snippets

<details>
  <summary>A function to play a sound effect</summary>

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
  <summary>How to initialize sounds</summary>

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

## Menu UI

### How its set up

The menu is set up with Unity's new input system, and it stops the game and changes to the UI inputs instead. You can also use either mouse or gamepad and sense which you are using when you pause the game, but you can also switch from mouse or gamepad while you have the menu up smoothly. 


![Ruin Menu](https://github.com/user-attachments/assets/3e1307bf-9993-4eda-8527-2182cfc4faf6)

### Code snippets

<details>
 <summary>How the pause function works</summary>

 ```
public void PauseUnPause()
{

    bool isPaused = pauseMenu.activeInHierarchy;
    if (!isPaused)
    {
        SoundFXManager.Instance.PlayButtonSoundFX(SoundType.ButtonSelect);
        isGamepad = playerInput.currentControlScheme == "Gamepad";
        playerInput.SwitchCurrentActionMap("UI");
        playerMovement.enabled = false;
        Hud_Ref.SetActive(true);
        panel.SetActive(true);
        goddesSymbol.SetActive(true);
        OpenMenu(pauseMenu);
        isPausing = true;
        Time.timeScale = 0f;
        justPaused = true;
        if (isGamepad && menuDefaultButtons.ContainsKey(currentMenu))
        {
            StartCoroutine(SetSelect(menuDefaultButtons[currentMenu]));
        }
        else
        {
            EventSystem.current.SetSelectedGameObject(null);
            Cursor.lockState = CursorLockMode.Confined;
            Cursor.visible = true;
            justPaused = false;
            isPausing = true;

        }
        playerCanvas.SetActive(false);
    }
    else if (isPaused)
    {
        SoundFXManager.Instance.PlayButtonSoundFX(SoundType.ButtonSelect);
        Cursor.lockState = CursorLockMode.Confined;
        Cursor.visible = false;
        playerMovement.enabled = true;
        playerInput.SwitchCurrentActionMap("Player");
        Time.timeScale = 1f;
        isPausing = false;
        pauseMenu.SetActive(false);
        optionsMenu.SetActive(false);
        soundMenu.SetActive(false);
        controllMenu.SetActive(false);
        Hud_Ref.SetActive(false);
        panel.SetActive(false);
        goddesSymbol.SetActive(false);
        currentMenu = null;

        playerCanvas.SetActive(true);

        menuHistory.Clear();
    }
}
```
</details>

<details>
<summary> How the button input set up works </summary>
 
```
private void OnClickPerformed(InputAction.CallbackContext context)
{
    var selected = EventSystem.current.currentSelectedGameObject;

    if (isPausing)
    {
        if (selected != null && selected.name == "ResumeButton")
        {
            ResumeGame();
        }
        else if (selected != null && selected.name == "OptionsButton")
        {
            Options();
        }
        else if (selected != null && selected.name == "QuitButton")
        {
            QuitGame();
        }
        else if (selected != null && selected.name == "BackButton")
        {
            BackButton();
        }
        else if (selected != null && selected.name == "BackButtonOptions")
        {
            BackButtonOptions();
        }
        else if (selected != null && selected.name == "SoundSettings")
        {
            SoundMenu();
        }
        else if (selected != null && selected.name == "ControllSettings")
        {
            ControllMenu();
        }
        else if (selected != null && selected.name == "BackButtonOptionsSound")
        {
            BackButtonOptions();
        }
        else
        {

        }
    }
}

```
 
</details>

 <details>
<summary>The Function to switch from mouse to gamepad</summary>

```
void OnControlsChanged(PlayerInput input)
{
    string scheme = input.currentControlScheme;
    isGamepad = scheme == "Gamepad";

    var uiActionMap = actions.FindActionMap("UI");
    var pointAction = uiActionMap?.FindAction("Point");

    if (pointAction != null)
    {
        if (isGamepad)
        {
            pointAction.Disable();
        }
        else
        {
            pointAction.Enable();
            Debug.Log("Mouse Point action re-enabled");
        }
    }

    // Reset selection
    EventSystem.current.SetSelectedGameObject(null);

    if (isGamepad)
    {
        if (currentMenu != null && menuDefaultButtons.ContainsKey(currentMenu))
        {
            EventSystem.current.SetSelectedGameObject(menuDefaultButtons[currentMenu]);
        }
        Cursor.visible = false;
        Cursor.lockState = CursorLockMode.Locked;
    }
    else
    {
        justPaused = false;
        Cursor.visible = true;
        Cursor.lockState = CursorLockMode.Confined;
    }
}

```
</details>
  
