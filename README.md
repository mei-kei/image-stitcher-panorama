# Basic Image Stitcher
This project is a Python-based computer vision tool that automatically stitches a series of overlapping images into a single panoramic image. It utilises the OpenCV library to handle feature detection, matching, and image warping.

The respective code is designed to run in a Jupyter Notebook environment (specifically Google Colab) and includes functionality to visualise the input sequence and save the final high-resolution result to Google Drive.

# Dependencies & Libraries
The script requires the following Python libraries:

- OpenCV (`cv2`): Used for image reading (`imread`), colour conversion (`cvtColor`), and the core stitching algorithms (`Stitcher_create`)
- Matplotlib (`matplotlib.pyplot`): Used for visualising the input images and the final result inline within the notebook
- Glob (`glob`): Used for retrieving file paths matching a specific pattern (e.g., `*.jpeg`)
- Math (`math`): Used for calculating grid dimensions when displaying input images.

# Setup and Configuration
Environment: The script is optimised for Google Colab
Image Source: Images should be stored in a Google Drive folder
Path Configuration:

  - Update the `images_path` variable to point to your input folder
  - Update the `output_path` variable to point to where you want the result saved.

# Step-by-Step Process of How It Works
**1. Image Loading and Preprocessing**
The scrippt uses `glob` to find all images in the target directory. It sorts them alphanumerically to make sure they're in the correct sequence.
  - **Colour Conversion**: OpenCV reads images in BGR (Blue-Green-Red) format by default. The script converts these to RGB immediately so they display correctly in Matplotlib.

**2. Visualisation**
Before processing, the script calculates a grid layout based on the number of input images and displays them using `matplotlib`. This allows the user to verify that the images are loaded in the correct order and have sufficient overlap.

**3. Stitching Process**
The logic utilises the `cv2.Stitcher_create()` class.
  - Feature Detection: the algorithm scans images for keypoints (corners, edges, high-contrast areas)
  - **Homography**: It calculates the geometric transformation required to align these keypoints across different images.
  - Blending: It warps the images and blends the seams to create a smooth composite.

**4. Status Check**
The operation returns a `status` code:

- `0` (cv2.Sticher_OK) would mean success
- `1` would mean that more images are required
- `2` or `3` would mean that the Homography/Camera estimation failed (usually due to lack of distinct features or insufficient overlap)

**5. Exporting Results**
If stitching is successful (`status == 0`)
- The script displays the result inline,
- It converts the RGB result back to **BGR**,
- It writes the file to the defined output path using `cv2.imwrite`.

# Troubleshooting
**"Stitching failed! Error code: 1"**
  - Cause: Not enough distinctive features found.
  - Fix: Make sure the images have at least 30%-50% overlap. Avoid stitching featureless walls or plain skies.

**Couldn't find a writer for the specified extension**
  - Cause: The output filename variable (`output_path`) doesn't end with a valid extension.
  - Fix: Make sure your output path ends in `.jpg` or `.png` (e.g., `panorama.jpg`)

**Colours look inverted (Blue people/Orange Sky)**
  - Cause: Mismatch between BGR and RGB colour spaces.
  - Fix: Make sure you convert to RGB for `plt.imshow` but convert back to BGR for `cv2.imwrite`.
