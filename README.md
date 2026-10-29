# Flowstate — AI Traffic Intelligence

Flowstate is a computer-vision project that detects and tracks vehicles in road footage, counts unique line crossings, and summarizes traffic volume and direction. The Streamlit dashboard turns the included sample run into an interactive traffic report.

## Dashboard

The dashboard includes:

- Traffic counts and peak 10-second interval
- Vehicle class distribution and directional totals
- A filterable crossing-event log with CSV downloads
- A short explanation of the YOLO → ByteTrack → line-crossing pipeline

The hosted dashboard displays the included precomputed sample analysis. It is not a live traffic feed. The Python scripts in `src/` rerun inference and generate new outputs locally.

## Run locally

Python 3.10 or newer is recommended. From the project root:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
streamlit run app/app.py
```

Open the local URL printed by Streamlit (usually `http://localhost:8501`). On macOS or Linux, activate the environment with `source .venv/bin/activate`.

## Run the computer-vision pipeline

Install the optional inference dependencies as well:

```powershell
python -m pip install -r requirements-vision.txt
```

Use the scripts from the repository root so their `data/`, model, and output paths resolve correctly. For example:

```powershell
python src/vehicle_counter.py
python src/traffic_statistics.py
```

The included sample input is `data/input/traffic.mp4`; the YOLO model is `yolo26n.pt`. Generated videos and intermediate tracking outputs are written under `runs/` and are excluded from Git. Small CSV summaries and charts in `runs/vehicle_counting/` are retained as dashboard sample data.

## Deploy on Streamlit Community Cloud

1. Push this repository to a GitHub repository.
2. In [Streamlit Community Cloud](https://share.streamlit.io/), choose **Create app** and connect the repository.
3. Select branch `main`, set the app file to `app/app.py`, and deploy.
4. Streamlit installs the dashboard packages from `requirements.txt`.

The dashboard needs no secrets or GPU. Running the YOLO pipeline on uploaded videos is a separate local workflow and is not part of the hosted sample dashboard.

## Project layout

```text
app/app.py                       Streamlit dashboard
src/                             Detection, tracking, and analytics scripts
data/input/traffic.mp4           Sample traffic footage
runs/vehicle_counting/*.csv      Precomputed dashboard data
requirements.txt                 Dashboard dependencies
requirements-vision.txt          Optional inference dependencies
```
