# Masoking
<img width="100%" height="600" alt="MasokingHeader" src="https://github.com/user-attachments/assets/56301df2-755e-4e44-bdb3-bace62480096" />

 <details>
  <summary>Details</summary>
   
Developed:  11/2024 - 01/2025  
Duration:   8 weeks  
Engine:     Unity  
Genre:      Bullet Hell 
Team:       3 Programmers, 4 Artist

  </details>
  
## Short Game Description  

You play as Masoking. A excentric masocist king who loves pain and plessure. 
He is ordering his jesters to cause him as much pain as possible, however the jesters don't really want to but does as he commands.
So the goal is to take as much damage as possible by the projectiles that are thrown by the jesters, but beware of your heatmeter if it goes low there is a chance that the king will become unsatisfied!

## My Roles in the project

During my time in the project I've helped with the movement, some off the jesters you encounter and also designed the music and soundeffects.
I have implemented a system to play sound effects and music, and I have also helped with the functionality off the menu.
The intro level is also designed by me, and I have also helped my fellow programmeres with different coding .

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

 <details>
<summary>
 How the jester wobbles
</summary>

```
private IEnumerator FallOver(GameObject gameObject, Vector2 playerPosition)
{
    hasDashed = true;


    Rigidbody2D rb = GetComponent<Rigidbody2D>();
    if (rb != null)
    {

        if (hasDashed != false && dashed != true)
        {
            _player.GetComponent<Player.Movement>().enabled = false;
            animator.SetBool("Wobble", true);
            dashed = true;
            // Temporarily disable Rigidbody2D to manually animate the fall
            rb.isKinematic = true;

            // Calculate the fall direction based on player position
            Vector2 impactDirection = (gameObject.transform.position - (Vector3)playerPosition).normalized;
            float fallAngle = impactDirection.x > 0 ? -90f : 90f; // Right or left fall
            float rotationTime = 0.5f; // Time it takes to fall
            Quaternion startRotation = gameObject.transform.rotation;
            Quaternion endRotation = Quaternion.Euler(0, 0, fallAngle);

            float wiggleAngle = 7;
            float wiggleCount = 1.5f;
            float wiggleDuration = 0.25f;

            lineRenderer.enabled = true;
            lineRenderer.useWorldSpace = true;

            triggerCollider.enabled = false;
            isFalling = true;

            if (impactDirection.x > 0) // Dash from left
            {
                lineRenderer.SetPosition(0, transform.position + transform.right * 3f);
                lineRenderer.SetPosition(1, transform.position);
            }
            else // Dash from right
            {
                lineRenderer.SetPosition(0, transform.position - transform.right * 3f);
            }

            // The second position stays anchored to the object
            lineRenderer.SetPosition(1, transform.position);

            lineRenderer.SetPosition(0,
                transform.position +
                Vector3.right * (impactDirection.x > 0 ? 2.4f : -2.4f)); // Adjust for dash side
            lineRenderer.SetPosition(1, transform.position);

            yield return new WaitForSeconds(0.2f);
            _player.GetComponent<Player.Movement>().enabled = true;

            for (int i = 0; i < wiggleCount; i++)
            {
                // Wiggle to the right
                Quaternion rightRotation = startRotation * Quaternion.Euler(0, 0, wiggleAngle);
                float elapsed = 0f;
                Quaternion leftRotation = startRotation * Quaternion.Euler(0, 0, -wiggleAngle);

                while (elapsed < wiggleDuration)
                {
                    elapsed += Time.deltaTime;

                    transform.rotation = Quaternion.Lerp(leftRotation, rightRotation, elapsed / wiggleDuration);

                    yield return null;
                }

                // Wiggle to the left
                elapsed = 0f;

                while (elapsed < wiggleDuration)
                {
                    elapsed += Time.deltaTime;

                    transform.rotation = Quaternion.Lerp(rightRotation, leftRotation, elapsed / wiggleDuration);

                    yield return null;
                }
            }


            // Rotate the object smoothly over time
            float elapsedTime = 0f;
            while (elapsedTime < rotationTime)
            {
                elapsedTime += Time.deltaTime;
                transform.rotation = Quaternion.Lerp(startRotation, endRotation, elapsedTime / rotationTime);
                yield return null;
            }

            // Ensure the final rotation matches exactly
            transform.rotation = endRotation;

            // Optionally re-enable physics
            rb.isKinematic = false;

            // (Optional) Freeze position constraints if it should stay down
            rb.constraints = RigidbodyConstraints2D.FreezeAll;

            lineRenderer.enabled = false;
            triggerCollider.enabled = true;
            isFalling = false;
            animator.SetBool("Wobble", false);
            animator.SetTrigger("Hit");
            SoundFXManager.Instance.PlayRandomSoundFX(smash, 2f);
            yield return new WaitForSeconds(0.1f);

            hasFallen = true;

            yield return new WaitForSeconds(2f);

            Destroy(rb.gameObject);
        }
    }
}
```
  
 </details>


### Bomb

The bomb works by having a timer until it sets off and the you need to be near it to take damage, and when you take damage you also launch away by the explosion impact

![Masoking Bomb](https://github.com/user-attachments/assets/eb1ba741-0af7-4262-9f55-b038d2073625)

<details>
 <summary>
  How to launch the player
 </summary>
 
```
if (_player)
{
    var sqrDistance = Vector3.SqrMagnitude(transform.position - _player.transform.position);

    if (sqrDistance <= explosionRadius)
    {
        _player.GetComponent<HeatSystem>().ChangeHeat(damage);


        _player.GetComponent<Player.Movement>().enabled = false;
        // Apply explosion force
        Rigidbody2D playerRb = _player.GetComponent<Rigidbody2D>();
        if (playerRb != null)
        {
            Vector2 forceDirection = _player.transform.position - transform.position;
             forceDirection.Normalize();
             float explosionForce = 2f; // Adjust as needed
            _player.GetComponent<Player.Movement>().Knocked(0.3f, forceDirection * explosionForce);
            yield return new WaitForSeconds(0.2f);
            _player.GetComponent<Player.Movement>().enabled = true;

        }


    }
}
```
 
</details>

## Sound and Music Manager

  ### How it works

It is set upp with different functions to play sounds, with options to play loops or play single sounds, you assign the sounds you want to play on the objects that you want to play sounds, and call the script in other codes where you want it to play, or stop if you use loops. some sound effects also change pitch based on how often they are played.

The music script is also set up real simple, just play different tracks for different levels. Theres also a function to lower the music if your heatmeter is depleting.

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

<details>

<summary> How the crossfade works </summary>

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Player;
using UnityEngine.Audio;

public class AudioCrossFade : MonoBehaviour
{
    //
    HeatSystem heat;

    private AudioSource normal;
    private AudioSource abnormal;

    public AudioMixerGroup mixer;
    public AudioClip normalClip;
    public AudioClip abnormalClip;

    public float overrideValue;

    // Start is called before the first frame update
    void Awake()
    {
        normal      = gameObject.AddComponent<AudioSource>();
        abnormal    = gameObject.AddComponent<AudioSource>();

        normal.clip = normalClip;
        abnormal.clip = abnormalClip;

        normal.outputAudioMixerGroup = mixer;
        abnormal.outputAudioMixerGroup = mixer;

        normal.loop = true;
        abnormal.loop = true;

    }

    private void Start()
    {
        normal.Play();
        abnormal.Play();
    }

    private float GetHeat01()
    {

        //return overrideValue;

       if(heat == null){
            heat = FindObjectOfType<HeatSystem>();
       }

        return heat.GetCurrentHeatNormalized();
    }

    private void Mute()
    {
        normal.volume = 0;

    }

    // Update is called once per frame
    void Update()
{
    normal.volume = GetHeat01();
    abnormal.volume = 1 - GetHeat01();
}
}

```
 
</details>

## Intro Level

### How its set up

You start the level by sitting down and not move, and a projectile misses you and you get sad, adn then a projectile hits you and can start to move.
When you get hit the board with the instructions move into screen and a teacher jester falls down to point at the instructions, after a while a new board comes in and tells you to dash into the door and start the level.

![Masoking Intro](https://github.com/user-attachments/assets/eb65acd1-0e18-4e25-b1bc-b6c1e6fd86a7)

<details>
 <summary> How the board moves  </summary>

```
private IEnumerator SwitchBoard()
{
    yield return new WaitForSeconds(3f);
    SoundFXManager.Instance.PlaySoundFX(sad,1f);
    MoveTextMoveLeft();
    yield return new WaitForSeconds(4f);
    MoveTextMoveRight();
    MoveBoardMoveRigth();
    MoveTextYeahMoveLeft();
    yield return new WaitForSeconds(3);
    MoveTextMoveYeahRight();

    yield return new WaitForSeconds(6f);
    MoveBoardMoveLeft();
    yield return new WaitForSeconds(2f);
    MoveBoardDash();

}

private IEnumerator MoveText()
{
    yield return new WaitForSeconds(7);

    

}

private void MoveBoardMoveRigth()
{
    rbMove.transform.DOMove(target2.position, 1.4f);
}

private void MoveBoardMoveLeft()
{
    rbMove.transform.DOMove(target1.position, 2f);
}

private void MoveTextMoveLeft()
{
    rbAw.transform.DOMove(targetLeftYeah.position, 2f);
}

private void MoveTextMoveRight()
{
    rbAw.transform.DOMove(targetRightYeah.position, 2f);
}

private void MoveTextYeahMoveLeft()
{
    rbYeah.transform.DOMove(targetLeftYeah.position, 1.3f);
}

private void MoveTextMoveYeahRight()
{
    rbYeah.transform.DOMove(targetRightYeah.position, 1.3f);
}


private void MoveBoardDash()
{
    rbDash.transform.DOMove(target2.position, 2f);
}
```
  
</details>

<details>
 <summary>
  How the jester falls
 </summary>

```
private void JesterFall()
{
  
    // Convert target3.position to Vector2
    Vector2 targetPosition = target3.position; // Implicitly converts Vector3 to Vector2 by ignoring the Z axis

    // Direction to target
    Vector2 direction = (targetPosition - rbTeacher.position).normalized;
    float gravityEffect = 20f; // Adjust gravity strength

    // Apply velocity with acceleration and gravity
    rbTeacher.velocity += acceleration * Time.deltaTime * direction;
    rbTeacher.velocity += gravityEffect * Time.deltaTime * Vector2.down  ;

    // Clamp speed to prevent it from getting too fast
    float maxSpeed = 10f;
    if (rbTeacher.velocity.magnitude > maxSpeed)
    {
        rbTeacher.velocity = rbTeacher.velocity.normalized * maxSpeed;
    }

    // Stop moving if close to the target
    if (Vector2.Distance(rbTeacher.position, targetPosition) < 0.1f)
    {
        rbTeacher.velocity = Vector2.zero;
        rbTeacher.MovePosition(targetPosition); // Snap the object to the target position
        if (VFx == false)
        {
            Instantiate(hitFloorVfxPrefab, rbTeacher.transform.position, Quaternion.identity);
            VFx = true;

        }
    }

}
```
</details>

  
