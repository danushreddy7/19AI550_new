# Ex.No: 9  Implementation of Simple Reinforcement Learning 
### DATE:                                                                            
### REGISTER NUMBER : 
### AIM: 
To write a program to implement  Reinforcement learning  in Unity

## Installation Required
```
1.Check sytem have python 3.10.0  ( if any higher version then uninstall and install python3.10.0)
2. Open commandprompt and Create and activate Python virtualenv by
     python -m venv venv 
     venv\Scripts\activate
3. install the packages 
   pip install numpy==1.23.5 scipy==1.10.1 h5py==3.8.0 protobuf==3.20.*
4. install ML agents by 
   pip install mlagents==0.28.0
5. install torch by 
  pip install torch torchvision torchaudio
6. Check mlagent version and check all the main options that you can use when launching the Python trainer by 
pip show mlagents 
mlagents-learn --help
```
### Algorithm:
```
1.Create a new 3D Unity project
2.Create a plane → Right-click Hierarchy > 3D Object > Plane
3.Create an Agent (Cube)
4.3D Object → Cube → Rename to Agent
5. Add Rigidbody (disable gravity if needed)
6. Create a Target (Sphere)
7.3D Object → Sphere → Rename to Target
8.Create an empty GameObject → Area (to reset Agent and Target positions)
9.Add the Behavior Parameters component to your Agent
10.Behavior Name: MoveToTarget
11. Vector Observation: 7 (3 for agent pos + 3 for target pos + 1 for velocity), 
Action Space: Continuous (2)
12. run the command 
mlagents-learn config.yaml --run-id=move-to-target --train
```  
### Program:
```
1. File : RollerAgent.cs 

using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    public Rigidbody rBody;
    public Transform targetTransform;
    public float forceMultiplier = 10f;

    public override void Initialize()
    {
        if (rBody == null) rBody = GetComponent<Rigidbody>();
    }

    public override void OnEpisodeBegin()
    {
        // Reset agent velocity and position
        rBody.velocity = Vector3.zero;
        rBody.angularVelocity = Vector3.zero;
        transform.localPosition = new Vector3(0, 0.5f, 0);

        // Move target to a random location on the plane
        float range = 3.0f;
        targetTransform.localPosition = new Vector3(Random.Range(-range, range), 0.5f, Random.Range(-range, range));
    }

    public override void CollectObservations(VectorSensor sensor)
    {
        // Target position (3 floats)
        sensor.AddObservation(targetTransform.localPosition);

        // Agent position (3 floats)
        sensor.AddObservation(transform.localPosition);

        // Agent velocity (2 floats: x,z)
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
        // -> total = 3 + 3 + 2 = 8
    }

    public override void OnActionReceived(ActionBuffers actions)
    {
        // Continuous actions: [0] = moveX, [1] = moveZ
        float moveX = Mathf.Clamp(actions.ContinuousActions[0], -1f, 1f);
        float moveZ = Mathf.Clamp(actions.ContinuousActions[1], -1f, 1f);

        Vector3 controlSignal = new Vector3(moveX, 0, moveZ);
        rBody.AddForce(controlSignal * forceMultiplier);

        // Reward shaping
        float distanceToTarget = Vector3.Distance(transform.localPosition, targetTransform.localPosition);

        // If agent reaches target -> give reward and end episode
        if (distanceToTarget < 1.5f)
        {
            SetReward(1.0f);
            EndEpisode();
        }

        // If falls off the plane -> negative reward and end episode
        if (transform.localPosition.y < -1f)
        {
            SetReward(-1.0f);
            EndEpisode();
        }

        // Small time penalty to encourage speed (optional)
        AddReward(-1f / MaxStep);
    }

    // For debugging: map keyboard input to actions
    public override void Heuristic(in ActionBuffers actionsOut)
    {
        var continuous = actionsOut.ContinuousActions;
        continuous[0] = Input.GetAxis("Horizontal");
        continuous[1] = Input.GetAxis("Vertical");
    }
}

2. Create a "Rollerball.yaml" file (create a Config folder inside your project ) attach the following code 

behaviors:
  RollerBallBehavior:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 10240
      learning_rate: 3.0e-4
      beta: 5.0e-3
      epsilon: 0.2
      lambd: 0.95
      num_epoch: 3
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```
### Output:
<img width="1918" height="1022" alt="image" src="https://github.com/user-attachments/assets/ab92693a-d53e-448b-a193-cd0fd8078f79" />

<img width="1918" height="1017" alt="image" src="https://github.com/user-attachments/assets/2c4c4ae9-4832-4abb-ba4d-e2bdca16befa" />

### Result:
Thus the AI character was trained using reinforcement learning.
