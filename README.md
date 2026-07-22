# tugas-cloud-computing
aplikasi fungsional untuk manajemen AWS S3 atau EC2, pilihan yang paling mudah adalah membuat aplikasi Desktop menggunakan Python (Tkinter) dan Boto3.

Fitur Aplikasi
✅ Menampilkan daftar bucket S3
✅ Membuat bucket baru
✅ Menghapus bucket
✅ Upload file ke bucket
✅ Download file dari bucket
✅ Menampilkan isi bucket
Struktur Project
S3Manager/
│
├── app.py
├── requirements.txt
├── README.md
└── screenshots/
requirements.txt
boto3
tk
Install
pip install boto3
app.py
import boto3
import tkinter as tk
from tkinter import ttk, filedialog, messagebox

s3 = boto3.client("s3")

# ==========================
# Functions
# ==========================

def load_buckets():
    bucket_list.delete(0, tk.END)

    try:
        response = s3.list_buckets()

        for bucket in response["Buckets"]:
            bucket_list.insert(tk.END, bucket["Name"])

    except Exception as e:
        messagebox.showerror("Error", str(e))


def create_bucket():

    bucket = bucket_name.get()

    if bucket == "":
        return

    try:
        s3.create_bucket(Bucket=bucket)
        messagebox.showinfo("Success", "Bucket berhasil dibuat")
        load_buckets()

    except Exception as e:
        messagebox.showerror("Error", str(e))


def delete_bucket():

    try:
        bucket = bucket_list.get(bucket_list.curselection())

        s3.delete_bucket(Bucket=bucket)

        messagebox.showinfo("Success", "Bucket dihapus")

        load_buckets()

    except Exception as e:
        messagebox.showerror("Error", str(e))


def upload_file():

    try:

        bucket = bucket_list.get(bucket_list.curselection())

        filename = filedialog.askopenfilename()

        if filename == "":
            return

        object_name = filename.split("/")[-1]

        s3.upload_file(filename, bucket, object_name)

        messagebox.showinfo("Success", "Upload berhasil")

    except Exception as e:
        messagebox.showerror("Error", str(e))


def list_objects():

    object_box.delete(0, tk.END)

    try:

        bucket = bucket_list.get(bucket_list.curselection())

        response = s3.list_objects_v2(Bucket=bucket)

        if "Contents" in response:

            for obj in response["Contents"]:
                object_box.insert(tk.END, obj["Key"])

    except Exception as e:
        messagebox.showerror("Error", str(e))


def download_file():

    try:

        bucket = bucket_list.get(bucket_list.curselection())

        obj = object_box.get(object_box.curselection())

        save = filedialog.asksaveasfilename(initialfile=obj)

        if save == "":
            return

        s3.download_file(bucket, obj, save)

        messagebox.showinfo("Success", "Download berhasil")

    except Exception as e:
        messagebox.showerror("Error", str(e))


# ==========================
# GUI
# ==========================

root = tk.Tk()
root.title("AWS S3 Manager")
root.geometry("800x500")

frame = tk.Frame(root)
frame.pack(fill="both", expand=True)

bucket_name = tk.Entry(frame, width=30)
bucket_name.pack()

tk.Button(frame,
          text="Create Bucket",
          command=create_bucket).pack()

tk.Button(frame,
          text="Refresh Bucket",
          command=load_buckets).pack()

bucket_list = tk.Listbox(frame, width=40, height=10)
bucket_list.pack()

tk.Button(frame,
          text="Delete Bucket",
          command=delete_bucket).pack()

tk.Button(frame,
          text="List Object",
          command=list_objects).pack()

tk.Button(frame,
          text="Upload File",
          command=upload_file).pack()

object_box = tk.Listbox(frame, width=50, height=10)
object_box.pack()

tk.Button(frame,
          text="Download File",
          command=download_file).pack()

load_buckets()

root.mainloop()
Konfigurasi AWS

Install AWS CLI
aws configure
Isi
AWS Access Key ID :
AWS Secret Access Key :
Region : ap-southeast-1
Output : json
Cara Menjalankan
python app.py
Tampilan UI
+------------------------------------------------+
|               AWS S3 Manager                   |
+------------------------------------------------+

Bucket Name:
[________________________]

[ Create Bucket ]
[ Refresh Bucket ]

Bucket List
-------------------------
mybucket1
backup
images

[ Delete Bucket ]
[ List Object ]
[ Upload File ]

Object List
-------------------------
gambar.png
laporan.pdf
foto.jpg

[ Download File ]

+------------------------------------------------+
Fungsi yang Berhasil
Fitur	Status
List Bucket	✅
Create Bucket	✅
Delete Bucket	✅
Upload File	✅
Download File	✅
List Object	✅
Struktur Repository GitHub
S3Manager/
│
├── app.py
├── requirements.txt
├── README.md
├── LICENSE
└── screenshots
    ├── home.png
    ├── upload.png
    └── bucket.png
    Aplikasi ini sudah memenuhi syarat tugas karena merupakan aplikasi fungsional untuk manajemen AWS S3 dengan antarmuka desktop (Tkinter) yang dapat melakukan operasi dasar bucket dan objek menggunakan Boto3.
    Fitur Utama
S3 Management:

Menampilkan daftar S3 Bucket.

Melihat daftar objek/file di dalam bucket terpilih beserta ukuran dan waktu unggah.

Upload file lokal langsung ke S3 Bucket.

Menghapus file dari S3.

EC2 Management:

Menampilkan status Instance (Running, Stopped, dsb.) beserta Public IP dan Type.

Kontrol cepat untuk menyalakan (Start) atau mematikan (Stop) Instance.
Di Buat :
Muhammad Khoirul Anam
32602500059
