# InkOnium
   <img width="1238" height="677" alt="Screenshot 2026-06-14 154720" src="https://github.com/user-attachments/assets/102a0934-9b67-4c6d-9ec4-b0f06de93f7d" />

 <details>
  <summary>Details</summary>

Developed:  3/2026 - Current  
Duration:   Still Going  
Engine:     Unity   
Genre:      VR,Survival,Adventure,Open World  
Team:       5 Programmers, 4 Artist  

  </details>
  
## Short Project Description  

A first person open world VR game, Where you get stranded on a desserted planet and have to repair your ship and get home.
The planet has a strange aura that you need to solve by gathering resources, build new tools and find out what made the planet turn this way by finding log books all while keeping your 
oxygine level in check to not die. While also find all the parts for your ship and make your way off the planet and report to the federation.

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/d9983592-c1f7-4431-8046-b3ac53c54236"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/43ad3fc7-51a8-40a2-a3f7-ab2df165acc7"></td>
    <td><img src="https://github.com/user-attachments/assets/6d289a7e-d3f6-46ff-a0b5-11eb4c94dd0f"></td>
  </tr>
</table>



---

## Cable Puzzle

### Fill Shader Graph

The Filling off the lights is a shader graph I created and changes the values in code

<table>
 <tr>
  <td><img src="https://github.com/user-attachments/assets/f63fb81f-b5fa-4556-af9d-66d792500ffd" alt="Fill Shader Graph" width="250" /></td>

   </tr>
</table>  
<details>
  <summary>How it works</summary>
This works by taking the world position and then uses the fill heigth variable to create a fill effects to "fill" it up, and you also uses the same world position to
for the glow effect.
   <table>
  <tr>  
  <td><img src="https://github.com/user-attachments/assets/94699893-3c95-4738-b91c-c71658bb61dd" alt="Screenshot 2026-06-14 163944" width="250" /></td>
  <td><img src="https://github.com/user-attachments/assets/72dd2ea3-07d8-44ae-8653-2c287024a41c" alt="Screenshot 2026-06-14 164005" width="250" /></td>
     </tr>
     <tr>
     <td><img src="https://github.com/user-attachments/assets/35ff6a04-8662-4ed1-8cb2-13d95e1459f6" alt="Screenshot 2026-06-14 164026" width="250" /></td>
     <td><img src="https://github.com/user-attachments/assets/de356499-5b89-4a7b-9020-85fa9a75ca34" alt="Screenshot 2026-06-14 164038" width="250" /></td>
   </tr>
</table>
<details>
  <summary>Code to control the fill height</summary>
 ``` 
public IEnumerator ChangeColor(float value, System.Action onFinished = null)
{
    // Delay to switch from overflow
    SwitchFromOverflow();
    float elapsed = 0;
    float start = fillHeight;

    float minFill = 0.9f;
    float maxFill = 2.12f;

    float totalSpheres = fillRenderers.Length;

    // Convert the number of spheres and normalizes it
    float normalized = value / totalSpheres;

    // Convert normalized → your shader range
    float currentValue = Mathf.Lerp(minFill, maxFill, normalized);


    float difference = Mathf.Abs(currentValue - start);

    // Define how fast the fill should move per second
    fillSpeed = 0.2f;

    // Duration that is dependent on distance
    float dynamicDuration = difference / fillSpeed;

    SoundFXManager.Instance.StartLoopFor(this.gameObject, SoundType.Fill, this.transform,0.8f);
    correctRenderMaterial.SetColor("_Fill_Color", fillColor);
    correctRenderMaterial.SetColor("_EmissionColor", emissionColorNormal);
    while (elapsed < dynamicDuration) 
    {
        elapsed += Time.deltaTime;

        blink = Mathf.PingPong(Time.time * fillSpeedBlink, 1);

        float t = elapsed / dynamicDuration;
        fillHeight = Mathf.Lerp(start, currentValue,t);
        for (int i = 0; i < fillRenderers.Length; i++)
        {
            Material m = renderMaterials[i];
            if (fillRenderers[i].transform.position.y < fillHeight)
            {
                m.SetFloat("_Fill_Height", fillHeight);
                m.SetColor("_Fill_Color", fillColor * blink);
                m.SetColor("_EmissionColor", emissionColorNormal * blink);
            }
            else
            {
                m.SetFloat("_Fill_Height", fillHeight);
                m.SetColor("_Fill_Color", fillColor * blink);
                m.SetColor("_EmissionColor", emissionColorNormal * blink);
            }
        }


        if (currentValue < start)
        {
            correctRenderMaterial.SetFloat("_Fill_Height", fillHeight /** 1.26f*/);
            SoundFXManager.Instance.ChangePitchForSoundObject(this.gameObject, -0.1f * Time.deltaTime);

        }
        else
        {
            SoundFXManager.Instance.ChangePitchForSoundObject(this.gameObject, 0.1f * Time.deltaTime);
        }

        yield return null;
    }

    for (int i = 0; i < fillRenderers.Length; i++)
    {
        Material m = renderMaterials[i];

        m.SetColor("_Fill_Color", fillColor);
        m.SetColor("_EmissionColor", emissionColorNormal);
    }

    SoundFXManager.Instance.StopLoopFor(this.gameObject);
    fillHeight = currentValue;
    onFinished?.Invoke();
}
  ``` 

   </details>
</details>

