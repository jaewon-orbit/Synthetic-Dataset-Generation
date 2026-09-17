# Synthetic Dataset Generation in Unity

A Unity script that captures a 3D object at a sequence of rotations to generate images for computer-vision training.

## Why I Built It

A friend needed many views of a particular object for a deep-learning project. I used Unity to automate image collection across a set of angles, producing a large batch of views without photographing each pose manually. I later shared the dataset on Hugging Face.

## How It Works

```text
CSV of yaw, pitch, roll → Rotate object in Unity → Render camera → Numbered PNG
```

[`StreamRotationFromCSV.cs`](StreamRotationFromCSV.cs) reads `ypr_data.csv` from `Application.persistentDataPath`, skips the header, and processes one pose per row. It applies `Quaternion.Euler(pitch, yaw, roll)` to the object, waits for the frame to finish, and saves a 512×512 camera image.

## My Contribution and Design Choices

I automated the rotation and capture loop in C#. An explicit list of poses made the capture sequence easy to control and reuse. Numbered output files preserve the order of the input rows.

The committed script rotates the object while capturing from an assigned camera. The angles come from the CSV rather than a fixed increment embedded in the script.

## Output

- [Published dataset on Hugging Face](https://huggingface.co/datasets/coding-Jay/Synthetic-Datasets-Unity-CV)
- [Related object-rotation estimation project](https://github.com/juicyjung/6DRotation_Estimation)

[![Dataset generation demo](https://img.youtube.com/vi/wjZhpO2m6Bk/0.jpg)](https://youtu.be/wjZhpO2m6Bk)

## Using the Script

1. Add the script to a GameObject in a Unity scene.
2. Assign `objectToRotate` and `cameraToCapture` in the Inspector.
3. Place a CSV named `ypr_data.csv` in `Application.persistentDataPath`, with a header and numeric yaw, pitch, and roll columns in that order.
4. Create the `NewDatasetProduced0` output directory relative to the working directory, or change the output path in the script.
5. Run the scene to save images as `1.png`, `2.png`, and so on.

## Scope

The repository contains the capture script. The original scene, object assets, and pose CSV are not included. Recreating the same views also requires the same camera, lighting, scene, and input poses. The script saves images; it does not export a separate annotation file or evaluate downstream model accuracy.
