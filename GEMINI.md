# Bioinformatic Fridays - Project Context

## Overview
This repository contains materials for **Bioinformatic Fridays**, a weekly training series designed for life scientists and bioinformatics trainees. The content focuses on foundational skills such as Linux command-line operations and Conda environment management.

## Directory Structure & Content

### Instructional Modules
The core content is organized into modules, each containing markdown lesson files:

*   **`Module_1_Linux/`**: Fundamentals of the Linux command line.
    *   **Focus:** Navigation, text processing, piping/redirection, basic scripting, permissions, and SSH.
    *   **Key Files:** `Lesson_1_...md` through `Lesson_4_...md`.
*   **`Module_2_Conda/`**: Package and environment management using Conda.
    *   **Focus:** Understanding Conda vs. pip, managing channels (bioconda, conda-forge), creating environments, and best practices for reproducibility.
    *   **Key Files:** `Lesson_1_...md` through `Lesson_5_...md`.

### Supporting Resources
*   **`Training/`**: Contains raw data files and practice exercises used in the lessons. (Note: This directory is typically git-ignored).
*   **`Slides/`**: Contains presentation slides for the weekly live sessions. (Note: This directory is typically git-ignored).

## Usage Guidelines
1.  **Sequential Learning:** Users are expected to proceed through modules and lessons in numerical order.
2.  **Hands-on Practice:** Lessons often reference data located in the `Training/` directory. Commands are intended to be run in a Unix-like terminal environment.
3.  **Environment:** The `Module_2_Conda` lessons guide the user in setting up a dedicated bioinformatics environment (`bioinfo`), emphasizing that the `base` environment should be kept clean.

## Important Configuration
*   **`.gitignore`**: Excludes `Slides`, `Training`, and specific instruction files from version control, suggesting these are local or generated resources.
