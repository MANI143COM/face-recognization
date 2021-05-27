{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "FACE EMOTION RECOGNITION",
      "provenance": [],
      "collapsed_sections": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "NTLUYIYKpQrr"
      },
      "source": [
        "# **IMAGE PREPROCESSING USING SKIMAGE AND OPENCV :**\n",
        "\n",
        "In this first part of the project we will be using SKimage, OpenCV and Matplotlib to get some insight about the data and preprocess it for further modeling.\n",
        "\n",
        "## **DataSet that We will be using:**\n",
        "\n",
        "Facial emotion recognition is the process of detecting human emotions from facial expressions. It has been a great source of communication between human beings, even they don't say a word. The human brain recognizes emotions automatically, and software has now been developed that can recognize emotions as well.\n",
        "\n",
        "The dataset we will be using is the one from kaggle, by Jonathan Oheix and here's the link for the <a href=\"https://www.kaggle.com/jonathanoheix/face-expression-recognition-dataset/activity\">dataset</a>.\n",
        "The dataset contains 35.9k files in total and contains images for different seven expressions which are labeled as:\n",
        "\n",
        "\n",
        "1.   Angry\n",
        "2.   Disgust\n",
        "1.   Fear\n",
        "2.   Happy\n",
        "1.   Neutral\n",
        "2.   Sad\n",
        "1.   Surprise\n",
        "\n",
        "And we will be  classifying the images of human faces among these 7 categories mentioned above.\n",
        "\n"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "hZdazUMTtUm0"
      },
      "source": [
        "<h3><b>Connecting the Google Drive to Google Colab</b></h3>\n",
        "\n",
        "The data we are using is already uploaded in the google drive. We need  to use the data into our colab network, so we have to connect our colab notebook to our drive so that we can access the data."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "irFPxVVSAO6h",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "2752bc47-1f68-47f8-c210-08a09e5f0b93"
      },
      "source": [
        "from google.colab import drive\n",
        "drive.mount('/content/drive')"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount(\"/content/drive\", force_remount=True).\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "nSyUGV0wuSkB"
      },
      "source": [
        "<h5>Import <b>os</b> so that we can use commands just like we use in a terminal in colab. </h5>"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MBVmS1A9SAwd"
      },
      "source": [
        "import os\n",
        "os.chdir('/content/drive/My Drive/FACIAL_EXPRESSION/Images')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "wzOcAgAdzaIW"
      },
      "source": [
        "<b>listdir</b> command is used to get list of all the files and folders present in the current working directory or the working directory which the passed path specify."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "EhI9n4DfSIYJ",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 151
        },
        "outputId": "ee86f113-a571-4099-acc7-9e26555204a8"
      },
      "source": [
        "os.listdir()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "['angry',\n",
              " 'surprise',\n",
              " 'happy',\n",
              " 'fear',\n",
              " 'neutral',\n",
              " 'sad',\n",
              " 'disgust',\n",
              " 'haarcascade_frontalface_alt.xml']"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 3
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "eBYq5x_WdXOY"
      },
      "source": [
        "#IMPORTING REQUIRED LIBERARIES\n",
        "\n",
        "import cv2\n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import skimage\n",
        "import skimage.transform"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "fXgVZ3CjBRsW"
      },
      "source": [
        "## FUNCTION TO SHOW IMAGE\n",
        "\n",
        "def image_show(img_path):\n",
        "  img = cv2.imread(img_path)\n",
        "  plt.imshow(img, cmap='gray')\n",
        "  plt.show()\n",
        "  print(f'The shape of the image is {img.shape}')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "t2crjFMABj6S",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 284
        },
        "outputId": "8235e730-c4bd-47f0-af1b-f2f1d88ea68e"
      },
      "source": [
        "image_show('/content/drive/My Drive/FACIAL_EXPRESSION/Images/disgust/10018.jpg') "
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        },
        {
          "output_type": "stream",
          "text": [
            "The shape of the image is (48, 48, 3)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "6qP3dclM0W1N"
      },
      "source": [
        "<H3><B>PREPROCESS</B></H3>\n",
        "\n",
        "Let's define a function to preprocess the image before passing it to the model.\n",
        "The processinf function has 3 subtasks:\n",
        "\n",
        "\n",
        "1.   READING THE IMAGE: We have used imread function of openCV for this.\n",
        "2.   Converting the RGb image to the grayscale image.\n",
        "3.   Resizing the image to a new size.\n",
        "\n",
        "The function at last return the processed image.\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "dbuN26Muctl6"
      },
      "source": [
        "## PREPROCESSING IMAGES\n",
        "\n",
        "def preprocess(img_path):\n",
        "  img = cv2.imread(img_path)\n",
        "  grey = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)\n",
        "  grey = skimage.transform.resize(grey, (42,42,1))\n",
        "  return grey"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "8MMQ5ucL2m2M"
      },
      "source": [
        "<B><H3> LOAD THE IMAGES </H3></B>\n",
        "\n",
        "We are loading 500 images of eaxh of the expressions and labeling them as numbers 0 to 6.\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "kf8ty6qFlGU6",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 185
        },
        "outputId": "714643df-b81c-4494-9ee5-327fb442b0fe"
      },
      "source": [
        "x = []   # image array\n",
        "y = []   # target labels\n",
        "\n",
        "for exp in os.listdir():\n",
        "  count = 0  ## STORES THE COUNT OF IMAGE OF EACH TYPE\n",
        "  print(f'Loading images for: {exp}')\n",
        "  if(exp=='angry'):\n",
        "    label=0\n",
        "  elif(exp=='surprise'):\n",
        "    label=1\n",
        "  elif(exp=='happy'):\n",
        "    label=2\n",
        "  elif(exp=='fear'):\n",
        "    label=3\n",
        "  elif(exp=='neutral'):\n",
        "    label=4\n",
        "  elif(exp=='sad'):\n",
        "    label=5\n",
        "  else:\n",
        "    label=6\n",
        "\n",
        "  try:\n",
        "    for img in os.listdir('./'+exp):\n",
        "      im = preprocess('./'+exp+'/'+img)\n",
        "      im = np.asarray(im)\n",
        "      x.append(im)\n",
        "      y.append(label)\n",
        "      count+=1\n",
        "      if(count==500):  \n",
        "        break  #MAKE SURE THAT WE LOAD ONLY 500 IMAGES FOR EACH OF THE EXPRESSIONS\n",
        "  except:\n",
        "    continue\n",
        "\n",
        "x = np.asarray(x)\n",
        "y = np.asarray(y)\n",
        "\n",
        "print(x.shape)\n",
        "print(y.shape)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Loading images for: angry\n",
            "Loading images for: surprise\n",
            "Loading images for: happy\n",
            "Loading images for: fear\n",
            "Loading images for: neutral\n",
            "Loading images for: sad\n",
            "Loading images for: disgust\n",
            "Loading images for: haarcascade_frontalface_alt.xml\n",
            "(3436, 42, 42, 1)\n",
            "(3436,)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "CKpOz6IIGoK_"
      },
      "source": [
        "<h2>Dummy Variables</h2>\n",
        "\n",
        "> <p>Columns like season, weathersit, mnth, hr, weekday contains finite discrete values which are all independent of each other. We need a way to represent these values such that the values remain independent of each other and these must be numbers.</p>\n",
        "\n",
        "The best way is to make a seperate column for each of the input value and represent values of each column as 0 or 1 depending on wether the input is that column or not.\n",
        "\n",
        "Let's see by an example what it means:\n",
        "\n",
        "![COLAB IMAGE.png](data:image/png;base64,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AAAAAIDpEXAAAAAAAADTI+AAAAAAAACmR8ABAAAAAABMj4ADAAAAAACYHgEHAAAAAAAwPQIOAAAAAABgegQcAAAAAADA9Ag4AAAAAACA6RFwAAAAAAAA0yPgAAAAAAAApkfAAQAAAAAATI+AAwAAAAAAmB4BBwAAAAAAMD0CDgAAAAAAYHoEHAAAAAAAwPQIOAAAAAAAgOkRcAAAAAAAANMj4AAAAAAAAKZHwAEAAAAAAEyPgAMAAAAAAJgeAQcAAAAAADA9Ag4AAAAAAGB6BBwAAAAAAMD0CDgAAAAAAIDpEXAAAAAAAADTI+AAAAAAAACmR8ABAAAAAABMj4ADAAAAAACYHgEHAAAAAAAwPQIOAAAAAABgegQcAAAAAADA9Ag4AAAAAACA6RFwAAAAAAAA0yPgAAAAAAAApkfAAQAAAAAATI+AAwAAAAAAmB4BBwAAAAAAMD0CDgAAAAAAYHoEHAAAAAAAwPQIOAAAAAAAgOkRcAAAAAAAANMj4AAAAAAAAKZHwAEAAAAAAEyPgAMAAAAAAJgeAQcAAAAAADA9Ag4AAAAAAGB6BBwAAAAAAMD0CDgAAAAAAIDpEXAAAAAAAADTI+AAAAAAAACmR8ABAAAAAABMj4ADAAAAAACYHgEHAAAAAAAwPQIOAAAAAABgegQcAAAAAADA9Ag4AAAAAACA6RFwAAAAAAAA0yPgAAAAAAAApkfAAQAAAAAATI+AAwAAAAAAmB4BBwAAAAAAMD0CDgAAAAAAYHoEHAAAAAAAwPQIOAAAAAAAgOkRcAAAAAAAANMj4AAAAAAAAKZHwAEAAAAAAEyPgAMAAAAAAJgeAQcAAAAAADA9Ag4AAAAAAGB6BBwAAAAAAMD0CDgAAAAAAIDpEXAAAAAAAADTI+AAAAAAAACmR8ABAAAAAABMj4ADAAAAAACYHgEHAAAAAAAwPQIOAAAAAABgegQcAAAAAADA9Ag4AAAAAACA6RFwAAAAAAAA0yPgAAAAAAAApkfAAQAAAAAATI+AAwAAAAAAmB4BBwAAAAAAMD0CDgAAAAAAYHoEHAAAAAAAwPQIOAAAAAAAgOkRcAAAAAAAANMj4AAAAAAAAKZHwAEAAAAAAEyPgAMAAAAAAJgeAQcAAAAAADA9Ag4AAAAAAGB6BBwAAAAAAMD0CDgAAAAAAIDpEXAAAAAAAADTI+AAAAAAAACm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bWxuczp4PSdhZG9iZTpuczptZXRhLyc+CjxyZGY6UkRGIHhtbG5zOnJkZj0naHR0cDovL3d3dy53My5vcmcvMTk5OS8wMi8yMi1yZGYtc3ludGF4LW5zIyc+CgogPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9JycKICB4bWxuczpBdHRyaWI9J2h0dHA6Ly9ucy5hdHRyaWJ1dGlvbi5jb20vYWRzLzEuMC8nPgogIDxBdHRyaWI6QWRzPgogICA8cmRmOlNlcT4KICAgIDxyZGY6bGkgcmRmOnBhcnNlVHlwZT0nUmVzb3VyY2UnPgogICAgIDxBdHRyaWI6Q3JlYXRlZD4yMDIwLTEwLTA2PC9BdHRyaWI6Q3JlYXRlZD4KICAgICA8QXR0cmliOkV4dElkPjAyZTc0ZThhLTdlN2EtNDU0Ny05YTc4LTM3MDk5ZTFmYzRlMjwvQXR0cmliOkV4dElkPgogICAgIDxBdHRyaWI6RmJJZD41MjUyNjU5MTQxNzk1ODA8L0F0dHJpYjpGYklkPgogICAgIDxBdHRyaWI6VG91Y2hUeXBlPjI8L0F0dHJpYjpUb3VjaFR5cGU+CiAgICA8L3JkZjpsaT4KICAgPC9yZGY6U2VxPgogIDwvQXR0cmliOkFkcz4KIDwvcmRmOkRlc2NyaXB0aW9uPgoKIDxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PScnCiAgeG1sbnM6cGRmPSdodHRwOi8vbnMuYWRvYmUuY29tL3BkZi8xLjMvJz4KICA8cGRmOkF1dGhvcj5BU0hJU0ggTU9ESTwvcGRmOkF1dGhvcj4KIDwvcmRmOkRlc2NyaXB0aW9uPgoKIDxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PScnCiAgeG1sbnM6eG1wPSdodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvJz4KICA8eG1wOkNyZWF0b3JUb29sPkNhbnZhPC94bXA6Q3JlYXRvclRvb2w+CiA8L3JkZjpEZXNjcmlwdGlvbj4KPC9yZGY6UkRGPgo8L3g6eG1wbWV0YT4KPD94cGFja2V0IGVuZD0ncic/PmFbqGkAAAAASUVORK5CYII=)\n",
        "\n"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "LrN3zimJ6QUK",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 134
        },
        "outputId": "793cff14-d95d-494b-b943-74eebd7498e6"
      },
      "source": [
        "from keras.utils import np_utils\n",
        "from sklearn.utils import shuffle\n",
        "\n",
        "y = np_utils.to_categorical(y,num_classes=7)\n",
        "x,y= shuffle(x,y,random_state=13)\n",
        "\n",
        "print(y)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "[[0. 0. 0. ... 0. 0. 1.]\n",
            " [0. 0. 0. ... 1. 0. 0.]\n",
            " [0. 0. 1. ... 0. 0. 0.]\n",
            " ...\n",
            " [1. 0. 0. ... 0. 0. 0.]\n",
            " [0. 0. 0. ... 1. 0. 0.]\n",
            " [1. 0. 0. ... 0. 0. 0.]]\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jVRNM6B07lO2",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "1b6878b7-0692-45f9-9c91-f6a28ec10d61"
      },
      "source": [
        "print(y.shape)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "(3436, 7)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "lphl2vJ6KS09"
      },
      "source": [
        "## Time to build the network\n",
        "\n",
        "\n",
        "TensorFlow is a free and open-source software library for dataflow and differentiable programming across a range of tasks. It is a symbolic math library, and is also used for machine learning applications such as neural networks.\n",
        "\n",
        "  "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jD0xs71R5zSf"
      },
      "source": [
        "import tensorflow as tf\n",
        "from tensorflow import keras\n",
        "from keras.models import Sequential\n",
        "from keras.layers import Dense, Conv2D, Dropout, Flatten, MaxPool2D"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5CfcJD6_vDwb"
      },
      "source": [
        "model = Sequential()\n",
        "model.add(Conv2D(32, kernel_size=(2,2), padding='same',input_shape=(42,42,1)))\n",
        "model.add(MaxPool2D(pool_size=(2, 2), strides=(2,2), padding=\"same\"))\n",
        "model.add(Conv2D(64, kernel_size=(2,2), padding='same'))\n",
        "model.add(MaxPool2D(pool_size=(2, 2), strides=(2,2), padding=\"same\"))\n",
        "model.add(Conv2D(128, kernel_size=(2,2), padding='same'))\n",
        "model.add(Flatten())\n",
        "model.add(Dense(128))\n",
        "model.add(Dropout(0.2))\n",
        "model.add(Dense(64))\n",
        "model.add(Dense(7, activation='softmax'))"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "GtkEXpyL7qsJ",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 487
        },
        "outputId": "171e1c40-dca7-4f4a-8842-5901625cbdd3"
      },
      "source": [
        "model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'], )\n",
        "model.summary()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Model: \"sequential_19\"\n",
            "_________________________________________________________________\n",
            "Layer (type)                 Output Shape              Param #   \n",
            "=================================================================\n",
            "conv2d_52 (Conv2D)           (None, 42, 42, 32)        160       \n",
            "_________________________________________________________________\n",
            "max_pooling2d_34 (MaxPooling (None, 21, 21, 32)        0         \n",
            "_________________________________________________________________\n",
            "conv2d_53 (Conv2D)           (None, 21, 21, 64)        8256      \n",
            "_________________________________________________________________\n",
            "max_pooling2d_35 (MaxPooling (None, 11, 11, 64)        0         \n",
            "_________________________________________________________________\n",
            "conv2d_54 (Conv2D)           (None, 11, 11, 128)       32896     \n",
            "_________________________________________________________________\n",
            "flatten_16 (Flatten)         (None, 15488)             0         \n",
            "_________________________________________________________________\n",
            "dense_45 (Dense)             (None, 128)               1982592   \n",
            "_________________________________________________________________\n",
            "dropout_29 (Dropout)         (None, 128)               0         \n",
            "_________________________________________________________________\n",
            "dense_46 (Dense)             (None, 64)                8256      \n",
            "_________________________________________________________________\n",
            "dense_47 (Dense)             (None, 7)                 455       \n",
            "=================================================================\n",
            "Total params: 2,032,615\n",
            "Trainable params: 2,032,615\n",
            "Non-trainable params: 0\n",
            "_________________________________________________________________\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "pMk-sL5X8S_r",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 202
        },
        "outputId": "4936a617-abba-4ae3-9c2f-fc7e990c7f93"
      },
      "source": [
        "model.fit(x, y, batch_size=64, epochs=5, validation_split=0.1)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Epoch 1/5\n",
            "49/49 [==============================] - 7s 144ms/step - loss: 2.3325 - accuracy: 0.1824 - val_loss: 1.8615 - val_accuracy: 0.2645\n",
            "Epoch 2/5\n",
            "49/49 [==============================] - 7s 142ms/step - loss: 1.7997 - accuracy: 0.2988 - val_loss: 1.7732 - val_accuracy: 0.3169\n",
            "Epoch 3/5\n",
            "49/49 [==============================] - 7s 141ms/step - loss: 1.7083 - accuracy: 0.3486 - val_loss: 1.7903 - val_accuracy: 0.2936\n",
            "Epoch 4/5\n",
            "49/49 [==============================] - 7s 141ms/step - loss: 1.6381 - accuracy: 0.3894 - val_loss: 1.7353 - val_accuracy: 0.3256\n",
            "Epoch 5/5\n",
            "49/49 [==============================] - 7s 140ms/step - loss: 1.5626 - accuracy: 0.4120 - val_loss: 1.6798 - val_accuracy: 0.3547\n"
          ],
          "name": "stdout"
        },
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "<tensorflow.python.keras.callbacks.History at 0x7f39ec3a7a58>"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 85
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cIo6gtOaQoZf"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    }
  ]
}
