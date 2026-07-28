{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "# Task 1: Data Understanding\n",
        "\n",
        "**Import Libraries**"
      ],
      "metadata": {
        "id": "rboKYfoa9PeR"
      }
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "metadata": {
        "id": "pr3ENlQu89Bc"
      },
      "outputs": [],
      "source": [
        "\n",
        "import pandas as pd\n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "\n",
        "from google.colab import files\n",
        "\n",
        "from sklearn.preprocessing import LabelEncoder\n",
        "from sklearn.preprocessing import StandardScaler\n",
        "\n",
        "from sklearn.cluster import KMeans\n",
        "from sklearn.decomposition import PCA"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "uploaded = files.upload()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 73
        },
        "id": "wUR7x1cK9ltJ",
        "outputId": "26be136f-f056-4d53-829c-0a78a981bd18"
      },
      "execution_count": 2,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<IPython.core.display.HTML object>"
            ],
            "text/html": [
              "\n",
              "     <input type=\"file\" id=\"files-03525c8d-17ff-4110-9851-4cffeddbf23c\" name=\"files[]\" multiple disabled\n",
              "        style=\"border:none\" />\n",
              "     <output id=\"result-03525c8d-17ff-4110-9851-4cffeddbf23c\">\n",
              "      Upload widget is only available when the cell has been executed in the\n",
              "      current browser session. Please rerun this cell to enable.\n",
              "      </output>\n",
              "      <script>// Copyright 2017 Google LLC\n",
              "//\n",
              "// Licensed under the Apache License, Version 2.0 (the \"License\");\n",
              "// you may not use this file except in compliance with the License.\n",
              "// You may obtain a copy of the License at\n",
              "//\n",
              "//      http://www.apache.org/licenses/LICENSE-2.0\n",
              "//\n",
              "// Unless required by applicable law or agreed to in writing, software\n",
              "// distributed under the License is distributed on an \"AS IS\" BASIS,\n",
              "// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.\n",
              "// See the License for the specific language governing permissions and\n",
              "// limitations under the License.\n",
              "\n",
              "/**\n",
              " * @fileoverview Helpers for google.colab Python module.\n",
              " */\n",
              "(function(scope) {\n",
              "function span(text, styleAttributes = {}) {\n",
              "  const element = document.createElement('span');\n",
              "  element.textContent = text;\n",
              "  for (const key of Object.keys(styleAttributes)) {\n",
              "    element.style[key] = styleAttributes[key];\n",
              "  }\n",
              "  return element;\n",
              "}\n",
              "\n",
              "// Max number of bytes which will be uploaded at a time.\n",
              "const MAX_PAYLOAD_SIZE = 100 * 1024;\n",
              "\n",
              "function _uploadFiles(inputId, outputId) {\n",
              "  const steps = uploadFilesStep(inputId, outputId);\n",
              "  const outputElement = document.getElementById(outputId);\n",
              "  // Cache steps on the outputElement to make it available for the next call\n",
              "  // to uploadFilesContinue from Python.\n",
              "  outputElement.steps = steps;\n",
              "\n",
              "  return _uploadFilesContinue(outputId);\n",
              "}\n",
              "\n",
              "// This is roughly an async generator (not supported in the browser yet),\n",
              "// where there are multiple asynchronous steps and the Python side is going\n",
              "// to poll for completion of each step.\n",
              "// This uses a Promise to block the python side on completion of each step,\n",
              "// then passes the result of the previous step as the input to the next step.\n",
              "function _uploadFilesContinue(outputId) {\n",
              "  const outputElement = document.getElementById(outputId);\n",
              "  const steps = outputElement.steps;\n",
              "\n",
              "  const next = steps.next(outputElement.lastPromiseValue);\n",
              "  return Promise.resolve(next.value.promise).then((value) => {\n",
              "    // Cache the last promise value to make it available to the next\n",
              "    // step of the generator.\n",
              "    outputElement.lastPromiseValue = value;\n",
              "    return next.value.response;\n",
              "  });\n",
              "}\n",
              "\n",
              "/**\n",
              " * Generator function which is called between each async step of the upload\n",
              " * process.\n",
              " * @param {string} inputId Element ID of the input file picker element.\n",
              " * @param {string} outputId Element ID of the output display.\n",
              " * @return {!Iterable<!Object>} Iterable of next steps.\n",
              " */\n",
              "function* uploadFilesStep(inputId, outputId) {\n",
              "  const inputElement = document.getElementById(inputId);\n",
              "  inputElement.disabled = false;\n",
              "\n",
              "  const outputElement = document.getElementById(outputId);\n",
              "  outputElement.innerHTML = '';\n",
              "\n",
              "  const pickedPromise = new Promise((resolve) => {\n",
              "    inputElement.addEventListener('change', (e) => {\n",
              "      resolve(e.target.files);\n",
              "    });\n",
              "  });\n",
              "\n",
              "  const cancel = document.createElement('button');\n",
              "  inputElement.parentElement.appendChild(cancel);\n",
              "  cancel.textContent = 'Cancel upload';\n",
              "  const cancelPromise = new Promise((resolve) => {\n",
              "    cancel.onclick = () => {\n",
              "      resolve(null);\n",
              "    };\n",
              "  });\n",
              "\n",
              "  // Wait for the user to pick the files.\n",
              "  const files = yield {\n",
              "    promise: Promise.race([pickedPromise, cancelPromise]),\n",
              "    response: {\n",
              "      action: 'starting',\n",
              "    }\n",
              "  };\n",
              "\n",
              "  cancel.remove();\n",
              "\n",
              "  // Disable the input element since further picks are not allowed.\n",
              "  inputElement.disabled = true;\n",
              "\n",
              "  if (!files) {\n",
              "    return {\n",
              "      response: {\n",
              "        action: 'complete',\n",
              "      }\n",
              "    };\n",
              "  }\n",
              "\n",
              "  for (const file of files) {\n",
              "    const li = document.createElement('li');\n",
              "    li.append(span(file.name, {fontWeight: 'bold'}));\n",
              "    li.append(span(\n",
              "        `(${file.type || 'n/a'}) - ${file.size} bytes, ` +\n",
              "        `last modified: ${\n",
              "            file.lastModifiedDate ? file.lastModifiedDate.toLocaleDateString() :\n",
              "                                    'n/a'} - `));\n",
              "    const percent = span('0% done');\n",
              "    li.appendChild(percent);\n",
              "\n",
              "    outputElement.appendChild(li);\n",
              "\n",
              "    const fileDataPromise = new Promise((resolve) => {\n",
              "      const reader = new FileReader();\n",
              "      reader.onload = (e) => {\n",
              "        resolve(e.target.result);\n",
              "      };\n",
              "      reader.readAsArrayBuffer(file);\n",
              "    });\n",
              "    // Wait for the data to be ready.\n",
              "    let fileData = yield {\n",
              "      promise: fileDataPromise,\n",
              "      response: {\n",
              "        action: 'continue',\n",
              "      }\n",
              "    };\n",
              "\n",
              "    // Use a chunked sending to avoid message size limits. See b/62115660.\n",
              "    let position = 0;\n",
              "    do {\n",
              "      const length = Math.min(fileData.byteLength - position, MAX_PAYLOAD_SIZE);\n",
              "      const chunk = new Uint8Array(fileData, position, length);\n",
              "      position += length;\n",
              "\n",
              "      const base64 = btoa(String.fromCharCode.apply(null, chunk));\n",
              "      yield {\n",
              "        response: {\n",
              "          action: 'append',\n",
              "          file: file.name,\n",
              "          data: base64,\n",
              "        },\n",
              "      };\n",
              "\n",
              "      let percentDone = fileData.byteLength === 0 ?\n",
              "          100 :\n",
              "          Math.round((position / fileData.byteLength) * 100);\n",
              "      percent.textContent = `${percentDone}% done`;\n",
              "\n",
              "    } while (position < fileData.byteLength);\n",
              "  }\n",
              "\n",
              "  // All done.\n",
              "  yield {\n",
              "    response: {\n",
              "      action: 'complete',\n",
              "    }\n",
              "  };\n",
              "}\n",
              "\n",
              "scope.google = scope.google || {};\n",
              "scope.google.colab = scope.google.colab || {};\n",
              "scope.google.colab._files = {\n",
              "  _uploadFiles,\n",
              "  _uploadFilesContinue,\n",
              "};\n",
              "})(self);\n",
              "</script> "
            ]
          },
          "metadata": {}
        },
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Saving Mall_Customers.csv to Mall_Customers.csv\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Display First Five Records**"
      ],
      "metadata": {
        "id": "_D35yBGx93B4"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df = pd.read_csv(next(iter(uploaded)))\n",
        "\n",
        "print(\"Dataset Loaded Successfully\")\n",
        "display(df.head())"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 223
        },
        "id": "PPNKL-lf-Gyk",
        "outputId": "f53bcb42-8db5-4706-b716-b1b470fd701d"
      },
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Dataset Loaded Successfully\n"
          ]
        },
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "   CustomerID  Gender  Age  Annual Income (k$)  Spending Score (1-100)\n",
              "0           1    Male   19                  15                      39\n",
              "1           2    Male   21                  15                      81\n",
              "2           3  Female   20                  16                       6\n",
              "3           4  Female   23                  16                      77\n",
              "4           5  Female   31                  17                      40"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-23ad18d6-0df9-4489-899c-f9ea442f3a2d\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>CustomerID</th>\n",
              "      <th>Gender</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>Male</td>\n",
              "      <td>19</td>\n",
              "      <td>15</td>\n",
              "      <td>39</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>2</td>\n",
              "      <td>Male</td>\n",
              "      <td>21</td>\n",
              "      <td>15</td>\n",
              "      <td>81</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>3</td>\n",
              "      <td>Female</td>\n",
              "      <td>20</td>\n",
              "      <td>16</td>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>4</td>\n",
              "      <td>Female</td>\n",
              "      <td>23</td>\n",
              "      <td>16</td>\n",
              "      <td>77</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>5</td>\n",
              "      <td>Female</td>\n",
              "      <td>31</td>\n",
              "      <td>17</td>\n",
              "      <td>40</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-23ad18d6-0df9-4489-899c-f9ea442f3a2d')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-23ad18d6-0df9-4489-899c-f9ea442f3a2d button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-23ad18d6-0df9-4489-899c-f9ea442f3a2d');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "summary": "{\n  \"name\": \"display(df\",\n  \"rows\": 5,\n  \"fields\": [\n    {\n      \"column\": \"CustomerID\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1,\n        \"min\": 1,\n        \"max\": 5,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          2,\n          5,\n          3\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Gender\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Female\",\n          \"Male\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 4,\n        \"min\": 19,\n        \"max\": 31,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          21,\n          31\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0,\n        \"min\": 15,\n        \"max\": 17,\n        \"num_unique_values\": 3,\n        \"samples\": [\n          15,\n          16\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 30,\n        \"min\": 6,\n        \"max\": 81,\n        \"num_unique_values\": 5,\n        \"samples\": [\n          81,\n          40\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Identify Numerical Features**"
      ],
      "metadata": {
        "id": "2KIAbfrN-L3Q"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "print(df.select_dtypes(include=np.number).columns)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "cLq7A8n0-WRE",
        "outputId": "f0617af2-bf8f-4968-a6d3-0879aec8d9d6"
      },
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Index(['CustomerID', 'Age', 'Annual Income (k$)', 'Spending Score (1-100)'], dtype='object')\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Identify Categorical Features**"
      ],
      "metadata": {
        "id": "_zw0wSQM-ZO9"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "print(df.select_dtypes(include='object').columns)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "_iILw_M9-dFG",
        "outputId": "6c26b6b3-38bf-471f-e02f-32954eb8d9e9"
      },
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Index(['Gender'], dtype='object')\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Dataset Information**"
      ],
      "metadata": {
        "id": "dFLz8Rui-u8N"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df.info()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "WnbDhoDf-5By",
        "outputId": "ba74319f-86b2-4a1e-a909-dd5196c38c46"
      },
      "execution_count": 8,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 200 entries, 0 to 199\n",
            "Data columns (total 5 columns):\n",
            " #   Column                  Non-Null Count  Dtype \n",
            "---  ------                  --------------  ----- \n",
            " 0   CustomerID              200 non-null    int64 \n",
            " 1   Gender                  200 non-null    object\n",
            " 2   Age                     200 non-null    int64 \n",
            " 3   Annual Income (k$)      200 non-null    int64 \n",
            " 4   Spending Score (1-100)  200 non-null    int64 \n",
            "dtypes: int64(4), object(1)\n",
            "memory usage: 7.9+ KB\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Summary Statistics**"
      ],
      "metadata": {
        "id": "ONuemRDc-8C0"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df.describe()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 300
        },
        "id": "f1eG7Tbp-_CS",
        "outputId": "f010425f-5a12-4ec0-c748-99adcbc78d88"
      },
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "       CustomerID         Age  Annual Income (k$)  Spending Score (1-100)\n",
              "count  200.000000  200.000000          200.000000              200.000000\n",
              "mean   100.500000   38.850000           60.560000               50.200000\n",
              "std     57.879185   13.969007           26.264721               25.823522\n",
              "min      1.000000   18.000000           15.000000                1.000000\n",
              "25%     50.750000   28.750000           41.500000               34.750000\n",
              "50%    100.500000   36.000000           61.500000               50.000000\n",
              "75%    150.250000   49.000000           78.000000               73.000000\n",
              "max    200.000000   70.000000          137.000000               99.000000"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-9ddfe320-e4d1-427f-b80c-03ea149ef480\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>CustomerID</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>200.000000</td>\n",
              "      <td>200.000000</td>\n",
              "      <td>200.000000</td>\n",
              "      <td>200.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>100.500000</td>\n",
              "      <td>38.850000</td>\n",
              "      <td>60.560000</td>\n",
              "      <td>50.200000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>57.879185</td>\n",
              "      <td>13.969007</td>\n",
              "      <td>26.264721</td>\n",
              "      <td>25.823522</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>1.000000</td>\n",
              "      <td>18.000000</td>\n",
              "      <td>15.000000</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>50.750000</td>\n",
              "      <td>28.750000</td>\n",
              "      <td>41.500000</td>\n",
              "      <td>34.750000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>100.500000</td>\n",
              "      <td>36.000000</td>\n",
              "      <td>61.500000</td>\n",
              "      <td>50.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>150.250000</td>\n",
              "      <td>49.000000</td>\n",
              "      <td>78.000000</td>\n",
              "      <td>73.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>200.000000</td>\n",
              "      <td>70.000000</td>\n",
              "      <td>137.000000</td>\n",
              "      <td>99.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-9ddfe320-e4d1-427f-b80c-03ea149ef480')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-9ddfe320-e4d1-427f-b80c-03ea149ef480 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-9ddfe320-e4d1-427f-b80c-03ea149ef480');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 8,\n  \"fields\": [\n    {\n      \"column\": \"CustomerID\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 71.77644232399086,\n        \"min\": 1.0,\n        \"max\": 200.0,\n        \"num_unique_values\": 6,\n        \"samples\": [\n          200.0,\n          100.5,\n          150.25\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 60.50135224289181,\n        \"min\": 13.969007331558883,\n        \"max\": 200.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          38.85,\n          36.0,\n          200.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 62.0103834127095,\n        \"min\": 15.0,\n        \"max\": 200.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          60.56,\n          61.5,\n          200.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 61.42496609345541,\n        \"min\": 1.0,\n        \"max\": 200.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          50.2,\n          50.0,\n          200.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 9
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "# Task 2: Data Preprocessing"
      ],
      "metadata": {
        "id": "e256F-zl_CDU"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df.isnull().sum()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 241
        },
        "id": "ifyAqpLp_HpF",
        "outputId": "1c751706-2266-4285-9cbf-c526d4a1a862"
      },
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "CustomerID                0\n",
              "Gender                    0\n",
              "Age                       0\n",
              "Annual Income (k$)        0\n",
              "Spending Score (1-100)    0\n",
              "dtype: int64"
            ],
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>0</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>CustomerID</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>Gender</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>Age</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div><br><label><b>dtype:</b> int64</label>"
            ]
          },
          "metadata": {},
          "execution_count": 10
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Remove CustomerID**"
      ],
      "metadata": {
        "id": "0ZTFbmfm_Mmv"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df = df.drop(\"CustomerID\", axis=1)\n",
        "\n",
        "df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "1PgPI7C5_KdW",
        "outputId": "1369d4b6-9098-4b99-963b-338a27ad5b11"
      },
      "execution_count": 11,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   Gender  Age  Annual Income (k$)  Spending Score (1-100)\n",
              "0    Male   19                  15                      39\n",
              "1    Male   21                  15                      81\n",
              "2  Female   20                  16                       6\n",
              "3  Female   23                  16                      77\n",
              "4  Female   31                  17                      40"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-e3e24788-4328-47dd-b342-b724732feac8\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Gender</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>Male</td>\n",
              "      <td>19</td>\n",
              "      <td>15</td>\n",
              "      <td>39</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>Male</td>\n",
              "      <td>21</td>\n",
              "      <td>15</td>\n",
              "      <td>81</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>Female</td>\n",
              "      <td>20</td>\n",
              "      <td>16</td>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>Female</td>\n",
              "      <td>23</td>\n",
              "      <td>16</td>\n",
              "      <td>77</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>Female</td>\n",
              "      <td>31</td>\n",
              "      <td>17</td>\n",
              "      <td>40</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-e3e24788-4328-47dd-b342-b724732feac8')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-e3e24788-4328-47dd-b342-b724732feac8 button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-e3e24788-4328-47dd-b342-b724732feac8');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "df",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 200,\n  \"fields\": [\n    {\n      \"column\": \"Gender\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 2,\n        \"samples\": [\n          \"Female\",\n          \"Male\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 13,\n        \"min\": 18,\n        \"max\": 70,\n        \"num_unique_values\": 51,\n        \"samples\": [\n          55,\n          26\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 26,\n        \"min\": 15,\n        \"max\": 137,\n        \"num_unique_values\": 64,\n        \"samples\": [\n          87,\n          101\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 25,\n        \"min\": 1,\n        \"max\": 99,\n        \"num_unique_values\": 84,\n        \"samples\": [\n          83,\n          39\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 11
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Encode Gender**"
      ],
      "metadata": {
        "id": "_64Kl0Qb_S9q"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "encoder = LabelEncoder()\n",
        "\n",
        "df[\"Gender\"] = encoder.fit_transform(df[\"Gender\"])\n",
        "\n",
        "df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "HVEOMejZ_VpH",
        "outputId": "3d27bb2e-6d51-44ce-aab1-bee484245ee0"
      },
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   Gender  Age  Annual Income (k$)  Spending Score (1-100)\n",
              "0       1   19                  15                      39\n",
              "1       1   21                  15                      81\n",
              "2       0   20                  16                       6\n",
              "3       0   23                  16                      77\n",
              "4       0   31                  17                      40"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-22618100-7dc4-45af-802c-d055fab7008e\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Gender</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>19</td>\n",
              "      <td>15</td>\n",
              "      <td>39</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1</td>\n",
              "      <td>21</td>\n",
              "      <td>15</td>\n",
              "      <td>81</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0</td>\n",
              "      <td>20</td>\n",
              "      <td>16</td>\n",
              "      <td>6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0</td>\n",
              "      <td>23</td>\n",
              "      <td>16</td>\n",
              "      <td>77</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>0</td>\n",
              "      <td>31</td>\n",
              "      <td>17</td>\n",
              "      <td>40</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-22618100-7dc4-45af-802c-d055fab7008e')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-22618100-7dc4-45af-802c-d055fab7008e button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-22618100-7dc4-45af-802c-d055fab7008e');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "df",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 200,\n  \"fields\": [\n    {\n      \"column\": \"Gender\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0,\n        \"min\": 0,\n        \"max\": 1,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          0,\n          1\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 13,\n        \"min\": 18,\n        \"max\": 70,\n        \"num_unique_values\": 51,\n        \"samples\": [\n          55,\n          26\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 26,\n        \"min\": 15,\n        \"max\": 137,\n        \"num_unique_values\": 64,\n        \"samples\": [\n          87,\n          101\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 25,\n        \"min\": 1,\n        \"max\": 99,\n        \"num_unique_values\": 84,\n        \"samples\": [\n          83,\n          39\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 12
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Standardize Features**"
      ],
      "metadata": {
        "id": "LcP7gAsj_YLd"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "scaler = StandardScaler()\n",
        "\n",
        "scaled_data = scaler.fit_transform(df)\n",
        "\n",
        "scaled_data[:5]"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Bv7lEVsy_bGA",
        "outputId": "bf5d7e93-84f8-4b1f-ce6d-0333ee081f5e"
      },
      "execution_count": 13,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "array([[ 1.12815215, -1.42456879, -1.73899919, -0.43480148],\n",
              "       [ 1.12815215, -1.28103541, -1.73899919,  1.19570407],\n",
              "       [-0.88640526, -1.3528021 , -1.70082976, -1.71591298],\n",
              "       [-0.88640526, -1.13750203, -1.70082976,  1.04041783],\n",
              "       [-0.88640526, -0.56336851, -1.66266033, -0.39597992]])"
            ]
          },
          "metadata": {},
          "execution_count": 13
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**# Task 3: Model Development**"
      ],
      "metadata": {
        "id": "ciW2KWd7_d1X"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "wcss = []\n",
        "\n",
        "for i in range(1,11):\n",
        "    model = KMeans(n_clusters=i, random_state=42, n_init=10)\n",
        "    model.fit(scaled_data)\n",
        "    wcss.append(model.inertia_)"
      ],
      "metadata": {
        "id": "OStPeJ1R_h61"
      },
      "execution_count": 14,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "plt.figure(figsize=(7,5))\n",
        "\n",
        "plt.plot(range(1,11), wcss, marker='o')\n",
        "\n",
        "plt.title(\"Elbow Method\")\n",
        "plt.xlabel(\"Number of Clusters\")\n",
        "plt.ylabel(\"WCSS\")\n",
        "\n",
        "plt.grid(True)\n",
        "\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 487
        },
        "id": "gG1Hi112_kaF",
        "outputId": "6c141900-b8c0-493a-c9ed-435b6768b1cf"
      },
      "execution_count": 15,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 700x500 with 1 Axes>"
            ],
            "image/png": "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ExEREbFTCmoiIiIidkpTSAFWqxWA7OxsgytxPPn5+Zw7d47s7GycnZ2NLkdKSP3meNRnjkn95njKos8u5o2L+eNqFNSA06dPAxASEmJwJSIiIlJenD59Gm9v76u2MVlLEuducUVFRRw7dgxPT09MJuMnYHUk2dnZhISEcPjwYby8vIwuR0pI/eZ41GeOSf3meMqiz6xWK6dPnyY4OBiz+ep3oWlEDTCbzVSrVs3oMhyal5eX/hByQOo3x6M+c0zqN8dT2n12rZG0i/QwgYiIiIidUlATERERsVMKavK3uLq6Mm7cOFxdXY0uRa6D+s3xqM8ck/rN8dhbn+lhAhERERE7pRE1ERERETuloCYiIiJipxTUREREROyUgpqIiIiInVJQkxsyceJEmjVrhqenJ/7+/vTt25fdu3cbXZZchzfffBOTycTIkSONLkWu4ejRowwePBg/Pz8qVKhAvXr12Lhxo9FlyRUUFhby0ksvER4eToUKFYiIiODVV18t0byOUnZWrlxJnz59CA4OxmQy8cMPPxRbb7VaefnllwkKCqJChQp06dKFvXv3lnmdCmpyQ1asWEFMTAzr1q0jNjaW/Px8unXrxtmzZ40uTUpgw4YN/Oc//6F+/fpGlyLXcOrUKVq3bo2zszO//voriYmJvP3221SqVMno0uQKJk2axPTp03n//ffZuXMnkyZNYvLkyUydOtXo0uQvzp49S4MGDZg2bdpl10+ePJkpU6bw4Ycfsn79ejw8POjevTs5OTllWqdezyE3xfHjx/H392fFihW0a9fO6HLkKs6cOUPjxo354IMPeO2112jYsCHvvfee0WXJFTz//POsWbOGVatWGV2KlFDv3r0JCAjg448/ti3r168fFSpUYPbs2QZWJldiMpmYN28effv2BS6MpgUHB/PMM8/w7LPPApCVlUVAQACzZs1iwIABZVabRtTkpsjKygLA19fX4ErkWmJiYujVqxddunQxuhQpgZ9++ommTZty77334u/vT6NGjfjoo4+MLkuuolWrVixZsoQ9e/YAsHXrVlavXk3Pnj0NrkxKKikpidTU1GJ/Tnp7e9OiRQvi4uLKtBZNyi5/W1FRESNHjqR169ZERUUZXY5cxZdffsmmTZvYsGGD0aVICR04cIDp06czevRoXnjhBTZs2MBTTz2Fi4sLQ4YMMbo8uYznn3+e7Oxs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TtnWb9682QpYk5KSrFar1VqvXj3rK6+8ctl9XW77nJwcq7u7u3Xt2rXF2g4dOtQ6cODAYtv98MMPxdp4enpaZ82adc1zEJG/z8mwhCgit4xJkybRqVOny44ilVTdunUxm/8c5A8ICCAqKsr22WKx4OfnR3p6erHtoqOjbf/u5ORE06ZN2blzJwBbt25l2bJlVKxY8ZLj7d+/n1q1agHQpEmTq9aWnZ3NsWPHaN26dbHlrVu3ZuvWrSU8wwv3eJWWp556imHDhrF48WK6dOlCv379qF+//hXb79u3j3PnztG1a9diy/Py8mjUqFGxZU2bNi32efTo0TzyyCN8/vnndOnShXvvvZeIiIibdzIiYqNLnyLyt7Vr147u3bszduzYS9aZzeZLAkp+fv4l7f730pnJZLrssqKiohLXdebMGfr06cOWLVuK/ezdu5d27drZ2l3uMmRpuO222zCZTOzateu6trsYYP/6Pf7vd/jII49w4MAB7r//frZt20bTpk2ZOnXqFfd55swZAH7++edi301iYmKx+9Tg0u/nlVdeYceOHfTq1YulS5cSGRnJvHnzruucRKRkFNRE5KZ48803mT9/PnFxccWWV6lShdTU1GIh42a+++yvN+AXFBSQkJBAnTp1AGjcuDE7duygevXq1KxZs9jP9YQzLy8vgoODWbNmTbHla9asITIyssT78fX1pXv37kybNo2zZ89esv5Kr8+oUqUKcOE+uIsu9x2GhITwxBNP8P333/PMM8/w0UcfAeDi4gJAYWGhrW1kZCSurq4kJydf8t2EhIRc81xq1arFqFGjWLx4MXffffdlH/QQkb9PQU1Ebop69eoxaNAgpkyZUmx5hw4dOH78OJMnT2b//v1MmzaNX3/99aYdd9q0acybN49du3YRExPDqVOnePjhh4ELN9hnZGQwcOBANmzYwP79+1m0aBEPPfRQsdBSEmPGjGHSpEl89dVX7N69m+eff54tW7bw9NNPX3e9hYWFNG/enO+++469e/eyc+dOpkyZUuwy7l9dDE+vvPIKe/fu5eeff+btt98u1mbkyJEsWrSIpKQkNm3axLJly2yBNSwsDJPJxIIFCzh+/DhnzpzB09OTZ599llGjRvHpp5+yf/9+Nm3axNSpU/n000+vWP/58+cZMWIEy5cv59ChQ6xZs4YNGzbYjiUiN5eCmojcNBMmTLjk0mSdOnX44IMPmDZtGg0aNCA+Pv5v3cv2v958803efPNNGjRowOrVq/npp5+oXLkygG0UrLCwkG7dulGvXj1GjhyJj49PsfvhSuKpp55i9OjRPPPMM9SrV4+FCxfy008/cdttt13XfmrUqMGmTZvo2LEjzzzzDFFRUXTt2pUlS5Ywffr0y27j7OzM3Llz2bVrF/Xr12fSpEm89tprxdoUFhYSExNDnTp16NGjB7Vq1eKDDz4AoGrVqowfP57nn3+egIAARowYAcCrr77KSy+9xMSJE23b/fzzz4SHh1+xfovFwsmTJ3nggQeoVasW/fv3p2fPnowfP/66vgcRKRmTtTTvbhURERGRG6YRNRERERE7paAmIiIiYqcU1ERERETslIKaiIiIiJ1SUBMRERGxUwpqIiIiInZKQU1ERETETimoiYiIiNgpBTURERERO6WgJiIiImKnFNRERERE7NT/Bz8PFbYj/ouhAAAAAElFTkSuQmCC\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Train K-Means**"
      ],
      "metadata": {
        "id": "LBBtUiiO_vC9"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "kmeans = KMeans(n_clusters=5, random_state=42, n_init=10)\n",
        "\n",
        "clusters = kmeans.fit_predict(scaled_data)"
      ],
      "metadata": {
        "id": "3U3UWgq6_xeg"
      },
      "execution_count": 16,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Assign Cluster Labels**"
      ],
      "metadata": {
        "id": "LtwwsLds_0s3"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "df[\"Cluster\"] = clusters\n",
        "\n",
        "df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "KlkauIJk_4ME",
        "outputId": "38eac09e-1a51-4753-dc12-8bbd806841d9"
      },
      "execution_count": 17,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   Gender  Age  Annual Income (k$)  Spending Score (1-100)  Cluster\n",
              "0       1   19                  15                      39        3\n",
              "1       1   21                  15                      81        3\n",
              "2       0   20                  16                       6        3\n",
              "3       0   23                  16                      77        3\n",
              "4       0   31                  17                      40        3"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-e0c9db70-c6b9-4d75-ae39-f0484697000b\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Gender</th>\n",
              "      <th>Age</th>\n",
              "      <th>Annual Income (k$)</th>\n",
              "      <th>Spending Score (1-100)</th>\n",
              "      <th>Cluster</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>1</td>\n",
              "      <td>19</td>\n",
              "      <td>15</td>\n",
              "      <td>39</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>1</td>\n",
              "      <td>21</td>\n",
              "      <td>15</td>\n",
              "      <td>81</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0</td>\n",
              "      <td>20</td>\n",
              "      <td>16</td>\n",
              "      <td>6</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>0</td>\n",
              "      <td>23</td>\n",
              "      <td>16</td>\n",
              "      <td>77</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>0</td>\n",
              "      <td>31</td>\n",
              "      <td>17</td>\n",
              "      <td>40</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-e0c9db70-c6b9-4d75-ae39-f0484697000b')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-e0c9db70-c6b9-4d75-ae39-f0484697000b button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-e0c9db70-c6b9-4d75-ae39-f0484697000b');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "df",
              "summary": "{\n  \"name\": \"df\",\n  \"rows\": 200,\n  \"fields\": [\n    {\n      \"column\": \"Gender\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0,\n        \"min\": 0,\n        \"max\": 1,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          0,\n          1\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Age\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 13,\n        \"min\": 18,\n        \"max\": 70,\n        \"num_unique_values\": 51,\n        \"samples\": [\n          55,\n          26\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Annual Income (k$)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 26,\n        \"min\": 15,\n        \"max\": 137,\n        \"num_unique_values\": 64,\n        \"samples\": [\n          87,\n          101\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Spending Score (1-100)\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 25,\n        \"min\": 1,\n        \"max\": 99,\n        \"num_unique_values\": 84,\n        \"samples\": [\n          83,\n          39\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Cluster\",\n      \"properties\": {\n        \"dtype\": \"int32\",\n        \"num_unique_values\": 5,\n        \"samples\": [\n          2,\n          1\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 17
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Apply PCA**"
      ],
      "metadata": {
        "id": "1iXYBl5Y_68-"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "pca = PCA(n_components=2)\n",
        "\n",
        "principal_components = pca.fit_transform(scaled_data)"
      ],
      "metadata": {
        "id": "S2t_bX-m__N6"
      },
      "execution_count": 18,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "**PCA DataFrame**"
      ],
      "metadata": {
        "id": "bN8kVqw2AC7b"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "pca_df = pd.DataFrame(\n",
        "    principal_components,\n",
        "    columns=[\"PC1\", \"PC2\"]\n",
        ")\n",
        "\n",
        "pca_df[\"Cluster\"] = clusters\n",
        "\n",
        "pca_df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "Nhp5xbYLAH9R",
        "outputId": "6221a22d-8760-4c8d-8d69-222149757d9c"
      },
      "execution_count": 19,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "        PC1       PC2  Cluster\n",
              "0 -0.406383 -0.520714        3\n",
              "1 -1.427673 -0.367310        3\n",
              "2  0.050761 -1.894068        3\n",
              "3 -1.694513 -1.631908        3\n",
              "4 -0.313108 -1.810483        3"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-1e5f2ab5-52d3-4cbc-9553-ca859402576e\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>PC1</th>\n",
              "      <th>PC2</th>\n",
              "      <th>Cluster</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>-0.406383</td>\n",
              "      <td>-0.520714</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>-1.427673</td>\n",
              "      <td>-0.367310</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>0.050761</td>\n",
              "      <td>-1.894068</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>-1.694513</td>\n",
              "      <td>-1.631908</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>-0.313108</td>\n",
              "      <td>-1.810483</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-1e5f2ab5-52d3-4cbc-9553-ca859402576e')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-1e5f2ab5-52d3-4cbc-9553-ca859402576e button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-1e5f2ab5-52d3-4cbc-9553-ca859402576e');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "pca_df",
              "summary": "{\n  \"name\": \"pca_df\",\n  \"rows\": 200,\n  \"fields\": [\n    {\n      \"column\": \"PC1\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1.1637756189648354,\n        \"min\": -2.148321588354201,\n        \"max\": 2.7742862260886647,\n        \"num_unique_values\": 200,\n        \"samples\": [\n          -0.5166629453678604,\n          -1.326130701573074,\n          2.547591941703813\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"PC2\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 1.0268876649710938,\n        \"min\": -2.023944788816325,\n        \"max\": 3.137256163241097,\n        \"num_unique_values\": 200,\n        \"samples\": [\n          0.8085829823343312,\n          -0.2367191493715475,\n          -0.5279135219026059\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Cluster\",\n      \"properties\": {\n        \"dtype\": \"int32\",\n        \"num_unique_values\": 5,\n        \"samples\": [\n          2,\n          1,\n          4\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 19
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**# Task 4: Visualization and Evaluation**"
      ],
      "metadata": {
        "id": "J0LpSb-vAK_W"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "plt.figure(figsize=(7,5))\n",
        "plt.plot(range(1,11), wcss, marker='o')\n",
        "plt.title(\"Elbow Curve\")\n",
        "plt.xlabel(\"Number of Clusters\")\n",
        "plt.ylabel(\"WCSS\")\n",
        "plt.grid(True)\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 487
        },
        "id": "-0zKFhAnASW-",
        "outputId": "44834646-a462-44a0-961f-b1499e0d1fd6"
      },
      "execution_count": 20,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 700x500 with 1 Axes>"
            ],
            "image/png": "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Cgp47733OHLkCHAhPGVkZPDHH3/Y2iQlJTF79uxrnv/fmc1m7r33XubOnct///tf8vLyLnmS8/777+fo0aO2Byv+7vz585w9e/a6jikiJUMjaiJimPnz57Nr165Llrdo0YKqVatedduEhAT+8Y9/0K1bN+Li4pgxYwb//Oc/bZf76tWrx8CBA/n0009JT0+nbdu2xMfHM336dHr37k379u0L7a9169Z888031KlTh/LlywPQsGFDypYty549e655f9pF7733Hvv37+fZZ5/lxx9/pGfPnpQvX57ExERmzZrFrl27bK/M6Nu3L//617+4++67efbZZzl37hxTpkyhRo0abNy4sUjHu+iBBx5g8uTJjBo1ijp16hQaRQQYMGAA3333HU899RRLly6lZcuW5Ofns2vXLr777jsWLlx42718WOS2YBURKWFTp061Alf8mTp1qq0tYB01apTt86hRo6yAdceOHdZ7773X6uHhYS1fvrx1yJAh1vPnzxc6Tm5urnX06NHWsLAwq7OzszU4ONg6cuRIa1ZW1iU1xcTEWAHr4MGDCy3v1KmTFbAuXry4yOeXl5dn/c9//mNt3bq11cvLy+rs7GwNDQ21PvLII9ZNmzYVarto0SJrZGSk1cXFxXrHHXdYZ8yYYTvHvwOs0dHRVzxmQUGBNTg42ApY33zzzcu2ycnJsY4fP95au3Ztq6urq7V8+fLWRo0aWUePHm3NyMgo8vmJSMkxWa1/ex23iIiIiNgN3aMmIiIiYqcU1ERERETslIKaiIiIiJ1SUBMRERGxUwpqIiIiInZKQU1ERETETumFt1x4W/ixY8fw8PDQ1CoiIiJSrKxWK6dPnyYoKAiz+epjZgpqwLFjxzQhsYiIiJSow4cPU7ly5au2UVADPDw8gAtfmKenp8HVOJbc3FwWLVpEly5dcHZ2NrocKSL1m+NRnzkm9ZvjKYk+y8zMJDg42JY/rkZBDWyXOz09PRXUrlNubi7u7u54enrqDyEHon5zPOozx6R+czwl2WdFud1KDxOIiIiI2CkFNRERERE7paAmIiIiYqcU1ERERETslIKaiIiIiJ1SUBMRERGxUwpqIiIiInZKQU1ERETETimoiYiIiNgpzUxQzPILrMQnpJF6Ogs/DzeahvlgMWvidxEREbk2Q0fU8vPzefXVVwkLC6NMmTKEh4fzxhtvYLVa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7dq1GT58OGvWrLnsNpcbEdu8eTMmk4mDBw8CcOjQIXr16kX58uUpW7YstWvX5tdff+XgwYO0b98egPLly2MymXj44Ydt5zR27FjCwsIoU6YM9erV4/vvv7/kuPPnz6dRo0a4urry+++/s2XLFtq3b4+Hhweenp40atSI9evXX7Z2Ebk5CmoicsMsFgtvv/02kydP5siRIze1ryVLlnDs2DFWrFjB+++/z6hRo+jZsyfly5dn7dq1PPXUUzz55JOXHGfEiBE8//zzbNq0iaioKHr16sXJkycBSE9Pp0OHDjRo0ID169ezYMECUlJSuP/++wvtY/r06bi4uLBq1So+/vjjy9b373//m/fee493332XP/74g65du/KPf/yDvXv3ApCUlETt2rV5/vnnSUpK4oUXXrhkH2lpaSxYsIDo6GjKli17yXpvb+8b+eoAiI6OJjs7mxUrVrB161bGjx9PuXLlCA4O5ocffgBg9+7dJCUl8e9//xuAsWPH8uWXX/Lxxx+zfft2hg0bRv/+/S8Juy+99BLjxo1j586d1K1bl379+lG5cmXWrVvHhg0beOmll3B2dr7h2kXkKqwiIjdg4MCB1rvuustqtVqtzZs3tz766KNWq9VqnT17tvXvf7SMGjXKWq9evULbTpw40RoaGlpoX6Ghodb8/HzbsjvuuMPaunVr2+e8vDxr2bJlrV9//bXVarVaExISrIB13Lhxtja5ubnWypUrW8ePH2+1Wq3WN954w9qlS5dCxz58+LAVsO7evdtqtVqtbdu2tTZo0OCa5xsUFGR96623Ci1r0qSJ9emnn7Z9rlevnnXUqFFX3MfatWutgPXHH3+85vEA6+zZs61Wq9W6dOlSK2A9deqUbf2mTZusgDUhIcFqtVqtderUsb7++uuX3dflts/KyrK6u7tbV69eXajtoEGDrA8++GCh7ebMmVOojYeHh3XatGnXPAcRuXlOhiVEEbltjB8/ng4dOlx2FKmoateujdn81yC/v78/kZGRts8WiwVfX19SU1MLbRcVFWX7dycnJxo3bszOnTsB2LJlC0uXLqVcuXKXHG///v3UqFEDgEaNGl21tszMTI4dO0bLli0LLW/ZsiVbtmwp4hleuMeruDz77LMMHjyYRYsW0alTJ/r06UPdunWv2H7fvn2cO3eOzp07F1qek5NDgwYNCi1r3Lhxoc/Dhw/nscce47///S+dOnXivvvuIzw8/NadjIjY6NKniNy0Nm3a0LVrV0aOHHnJOrPZfElAyc3NvaTd/146M5lMl11WUFBQ5LrOnDlDr1692Lx5c6GfvXv30qZNG1u7y12GLA7Vq1fHZDKxa9eu69ruYoD9+/f4v9/hY489xoEDBxgwYABbt26lcePGTJ48+Yr7PHPmDAC//PJLoe9mx44dhe5Tg0u/n9dff53t27fTo0cPlixZQkREBLNnz76ucxKRolFQE5FbYty4ccydO5e4uLhCyytWrEhycnKhkHEr33329xvw8/Ly2LBhA7Vq1QKgYcOGbN++nSpVqlCtWrVCP9cTzjw9PQkKCmLVqlWFlq9atYqIiIgi78fHx4euXbsSExPD2bNnL1l/pddnVKxYEbhwH9xFl/sOg4ODeeqpp/jxxx95/vnn+eyzzwBwcXEBID8/39Y2IiICV1dXEhMTL/lugoODr3kuNWrUYNiwYSxatIh77rnnsg96iMjNU1ATkVuiTp069OvXj0mTJhVa3q5dO44fP86ECRPYv38/MTExzJ8//5YdNyYmhtmzZ7Nr1y6io6M5deoUjz76KHDhBvu0tDQefPBB1q1bx/79+1m4cCGPPPJIodBSFCNGjGD8+PF8++237N69m5deeonNmzfz3HPPXXe9+fn5NG3alB9++IG9e/eyc+dOJk2aVOgy7t9dDE+vv/46e/fu5ZdffuG9994r1Gbo0KEsXLiQhIQENm7cyNKlS22BNTQ0FJPJxLx58zh+/DhnzpzBw8ODF154gWHDhjF9+nT279/Pxo0bmTx5MtOnT79i/efPn2fIkCEsW7aMQ4cOsWrVKtatW2c7lojcWgpqInLLjBkz5pJLk7Vq1eKjjz4iJiaGevXqER8ff1P3sv2vcePGMW7cOOrVq8fvv//Ozz//TIUKFQBso2D5+fl06dKFOnXqMHToULy9vQvdD1cUzz77LMOHD+f555+nTp06LFiwgJ9//pnq1atf136qVq3Kxo0bad++Pc8//zyRkZF07tyZxYsXM2XKlMtu4+zszNdff82uXbuoW7cu48eP58033yzUJj8/n+joaGrVqkW3bt2oUaMGH330EQCVKlVi9OjRvPTSS/j7+zNkyBAA3njjDV599VXGjh1r2+6XX34hLCzsivVbLBZOnjzJQw89RI0aNbj//vvp3r07o0ePvq7vQUSKxmQtzrtbRUREROSGaURNRERExE4pqImIiIjYKQU1ERERETuloCYiIiJipxTUREREROyUgpqIiIiInVJQExEREbFTCmoiIiIidkpBTURERMROKaiJiIiI2CkFNRERERE79f8aBfHIpF7Y3wAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "plt.figure(figsize=(8,6))\n",
        "\n",
        "plt.scatter(\n",
        "    pca_df[\"PC1\"],\n",
        "    pca_df[\"PC2\"],\n",
        "    c=pca_df[\"Cluster\"],\n",
        "    cmap=\"viridis\",\n",
        "    s=60\n",
        ")\n",
        "\n",
        "plt.title(\"Customer Segments using PCA\")\n",
        "plt.xlabel(\"Principal Component 1\")\n",
        "plt.ylabel(\"Principal Component 2\")\n",
        "plt.colorbar(label=\"Cluster\")\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 564
        },
        "id": "fRAqbeQRAaat",
        "outputId": "b04cde78-63f4-4142-de77-9e55f8a0cce1"
      },
      "execution_count": 21,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 800x600 with 2 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAp8AAAIjCAYAAABF4HAGAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQABAABJREFUeJzs3Xd8U1UbwPHfvUn3bhkFyt57owVkiGxlKMsFOHAB6utGQUBBxL0ZKqACiiBD2UOGCih7iKDs3TK6d3PP+0dooXQkadP9fN9PPq+9OTn3SUmTJ+ee8xxNKaUQQgghhBCiAOiFHYAQQgghhCg9JPkUQgghhBAFRpJPIYQQQghRYCT5FEIIIYQQBUaSTyGEEEIIUWAk+RRCCCGEEAVGkk8hhBBCCFFgJPkUQgghhBAFRpJPIYQQQghRYCT5FEKIEmbOnDlomsbJkycLOxQhhMhEkk9RIh07dozHH3+cGjVq4O7ujq+vL+3atePjjz8mISEhX845f/58Pvroo3zpuyg4cOAAAwYMoGrVqri7u1OpUiW6du3Kp59+WtihFbjz588zYcIE9u7dW9ihFDkTJkxA07T0m6enJw0aNGDs2LFER0dnau/o36rFYqFixYpomsaqVasK4ikJIZxMk73dRUmzYsUKBg4ciJubG0OHDqVRo0YkJyfz+++/89NPPzF8+HBmzpzp9PPeeeedHDx4sESONm3dupXOnTtTpUoVhg0bRnBwMGfOnGH79u0cO3aMo0ePFnaIBWrnzp20bt2a2bNnM3z48MIOJxOLxUJKSgpubm5omlag554wYQITJ05k2rRpeHt7Exsby9q1a1myZAmhoaH88ccf6THl5m913bp1dOvWjWrVqtGuXTvmzp1boM9PCJF35sIOQAhnOnHiBEOGDKFq1ar8+uuvVKhQIf2+kSNHcvToUVasWFGIERZdcXFxeHl5ZXnf5MmT8fPzY8eOHfj7+2e4Lzw8vACiE44wmUyYTKZCjWHAgAGUKVMGgCeeeIJ77rmHxYsXs337dkJDQ3P9tzp37lxatGjBsGHDePXVV3N83Qohiia57C5KlHfeeYfY2Fi+/vrrDB9maWrVqsUzzzwDwMmTJ9E0jTlz5mRqp2kaEyZMSP85JiaGZ599lmrVquHm5ka5cuXo2rUru3fvBqBTp06sWLGCU6dOpV9urFatWvrjw8PDeeSRRyhfvjzu7u40bdqUb775JsM50+J57733+Pzzz6lRowaenp5069aNM2fOoJTizTffJCQkBA8PD/r27cvVq1czxb5q1Spuu+02vLy88PHxoXfv3vz9998Z2gwfPhxvb2+OHTtGr1698PHx4f7778/293rs2DEaNmyYKfEEKFeuXKZjc+fOpWXLlnh4eBAYGMiQIUM4c+ZMpnZpz9PDw4M2bdrw22+/0alTJzp16pTeZtOmTWiaxo8//sjEiROpVKkSPj4+DBgwgKioKJKSknj22WcpV64c3t7ePPTQQyQlJeUqpk6dOtGoUSMOHTpE586d8fT0pFKlSrzzzjsZ4mndujUADz30UPq/d9rr6L///uOee+4hODgYd3d3QkJCGDJkCFFRUdn+fgGqVauW5Sjqzb8PgE8//ZSGDRvi6elJQEAArVq1Yv78+en3ZzXns1q1atx55538/vvvtGnTBnd3d2rUqMG3336b6Zz79++nY8eOeHh4EBISwqRJk5g9e3ae5pHefvvtgPULIjj2t5omISGBJUuWMGTIEAYNGkRCQgLLli3LVTxCiMIjI5+iRPnll1+oUaMGbdu2dWq/TzzxBIsWLWLUqFE0aNCAK1eu8Pvvv/PPP//QokULXnvtNaKiojh79iwffvghAN7e3oD1A7NTp04cPXqUUaNGUb16dRYuXMjw4cOJjIzM9AE7b948kpOTGT16NFevXuWdd95h0KBB3H777WzatImXX36Zo0eP8umnn/LCCy8wa9as9Md+9913DBs2jO7duzN16lTi4+OZNm0a7du3Z8+ePRkS4tTUVLp370779u1577338PT0zPb5V61alW3btnHw4EEaNWqU4+9q8uTJjBs3jkGDBvHoo49y6dIlPv30Uzp06MCePXvSE9hp06YxatQobrvtNv73v/9x8uRJ+vXrR0BAACEhIZn6nTJlCh4eHrzyyivpz9/FxQVd14mIiGDChAls376dOXPmUL16dV5//XWHYwKIiIigR48e3H333QwaNIhFixbx8ssv07hxY3r27En9+vV54403eP3113nssce47bbbAGjbti3Jycl0796dpKQkRo8eTXBwMOfOnWP58uVERkbi5+eX4+/OHl9++SVPP/00AwYM4JlnniExMZH9+/fz559/ct999+X42KNHjzJgwAAeeeQRhg0bxqxZsxg+fDgtW7akYcOGAJw7d47OnTujaRpjxozBy8uLr776Cjc3tzzFfezYMQCCgoKA3P2t/vzzz8TGxjJkyBCCg4Pp1KkT8+bNs/m8hRBFjBKihIiKilKA6tu3r13tT5w4oQA1e/bsTPcBavz48ek/+/n5qZEjR+bYX+/evVXVqlUzHf/oo48UoObOnZt+LDk5WYWGhipvb28VHR2dIZ6yZcuqyMjI9LZjxoxRgGratKlKSUlJP37vvfcqV1dXlZiYqJRSKiYmRvn7+6sRI0ZkOP/FixeVn59fhuPDhg1TgHrllVdyfE5p1q5dq0wmkzKZTCo0NFS99NJLas2aNSo5OTlDu5MnTyqTyaQmT56c4fiBAweU2WxOP56UlKSCgoJU69atMzynOXPmKEB17Ngx/djGjRsVoBo1apThfPfee6/SNE317Nkzw7lCQ0Mz/DvYG5NSSnXs2FEB6ttvv00/lpSUpIKDg9U999yTfmzHjh1Zvnb27NmjALVw4cKsfo05qlq1qho2bFim4x07dszw++jbt69q2LBhjn3Nnj1bAerEiRMZ+gfUli1b0o+Fh4crNzc39fzzz6cfGz16tNI0Te3Zsyf92JUrV1RgYGCmPrMyfvx4BagjR46oS5cuqRMnTqgZM2YoNzc3Vb58eRUXF+fw32qaO++8U7Vr1y7955kzZyqz2azCw8Md6kcIUbjksrsoMdJW0vr4+Di9b39/f/7880/Onz/v8GNXrlxJcHAw9957b/oxFxcXnn76aWJjY9m8eXOG9gMHDswwQnbLLbcA8MADD2A2mzMcT05O5ty5c4B1IUZkZCT33nsvly9fTr+ZTCZuueUWNm7cmCm2J5980q7n0LVrV7Zt20afPn3Yt28f77zzDt27d6dSpUr8/PPP6e0WL16MYRgMGjQoQwzBwcHUrl07PYadO3dy5coVRowYkeE53X///QQEBGQZw9ChQ3Fxccnw/JVSPPzwwxna3XLLLZw5c4bU1FSHYkrj7e3NAw88kP6zq6srbdq04fjx4zZ/T2n/bmvWrCE+Pt5m+9zw9/fn7Nmz7Nixw+HHNmjQIH2kFqBs2bLUrVs3w3NbvXo1oaGhNGvWLP1YYGBgjtMyslK3bl3Kli1L9erVefzxx6lVqxYrVqzA09MzV3+rV65cYc2aNRn+ju655570KRlCiOJDLruLEsPX1xewzs90tnfeeYdhw4ZRuXJlWrZsSa9evRg6dCg1atSw+dhTp05Ru3ZtdD3jd7369eun33+jKlWqZPg5LaGpXLlylscjIiIA61xDuD637mZpv580ZrM5y8vb2WndujWLFy8mOTmZffv2sWTJEj788EMGDBjA3r17adCgAf/99x9KKWrXrp1lH2nJY9pzrlWrVqaYbpwacCNHfi+GYRAVFUVQUJDdMaUJCQnJtEI8ICCA/fv3Z/n4G1WvXp3nnnuODz74gHnz5nHbbbfRp08fHnjgAadccgd4+eWXWb9+PW3atKFWrVp069aN++67j3bt2tl87M2/Q7A+t7TXEFj/bUJDQzO1u/nfypaffvoJX19fXFxcCAkJoWbNmun35eZvdcGCBaSkpNC8efMM1RVuueUW5s2bx8iRIx2KTwhReCT5FCWGr68vFStW5ODBg3a1z64EjcViyXRs0KBB3HbbbSxZsoS1a9fy7rvvMnXqVBYvXkzPnj3zFPfNslulnN1xda1ammEYgHXeZ3BwcKZ2N44wAri5uWVKiO3h6upK69atad26NXXq1OGhhx5i4cKFjB8/HsMw0usvZhVv2jzY3MjL78WRmGz1Z8v777/P8OHDWbZsGWvXruXpp59mypQpbN++PcdkP6fX440x1a9fnyNHjrB8+XJWr17NTz/9xBdffMHrr7/OxIkTc4wtr8/NER06dEhf7X4zR/9WwToXGsg2yT5+/LhdXwaFEIVPkk9Rotx5553MnDmTbdu2ZTl6c6O0y7uRkZEZjt88EpmmQoUKPPXUUzz11FOEh4fTokULJk+enJ58Zpc8VK1alf3792MYRoZk7/Dhw+n3O0PayFK5cuW44447nNKnLa1atQLgwoUL6TEopahevTp16tTJ9nFpz/no0aN07tw5/XhqaionT56kSZMmTovR3pgcYat2ZuPGjWncuDFjx45l69attGvXjunTpzNp0qRsHxMQEJDptQjW1+PNSZWXlxeDBw9m8ODBJCcnc/fddzN58mTGjBmDu7t7rp5TmqpVq2ZZt9XZtVwd+Vs9ceIEW7duZdSoUXTs2DHDfYZh8OCDDzJ//nzGjh3r1BiFEPlD5nyKEuWll17Cy8uLRx99lLCwsEz3Hzt2jI8//hiwjr6UKVOGLVu2ZGjzxRdfZPjZYrFkKpNTrlw5KlasmKGkj5eXV5bldHr16sXFixdZsGBB+rHU1FQ+/fRTvL29M32Y5lb37t3x9fXlrbfeIiUlJdP9ly5dynXfGzduzHJ0bOXKlYB1fh/A3XffjclkYuLEiZnaK6W4cuUKYE1ag4KC+PLLL9PnZoJ1dOvGS8DOYG9MjkirK3lzshgdHZ3h+YA1EdV1PcvyTzeqWbMm27dvJzk5Of3Y8uXLM5WDujleV1dXGjRogFIqy393R3Xv3p1t27Zl2L3p6tWr6SOPzuLI32rauV966SUGDBiQ4TZo0CA6duzo9PiEEPlHRj5FiVKzZk3mz5/P4MGDqV+/foZdU7Zu3Zpe4ijNo48+yttvv82jjz5Kq1at2LJlC//++2+GPmNiYggJCWHAgAE0bdoUb29v1q9fz44dO3j//ffT27Vs2ZIFCxbw3HPP0bp1a7y9vbnrrrt47LHHmDFjBsOHD2fXrl1Uq1aNRYsW8ccff/DRRx85bYGUr68v06ZN48EHH6RFixYMGTKEsmXLcvr0aVasWEG7du347LPPctX36NGjiY+Pp3///tSrVy/997lgwQKqVavGQw89BFh//5MmTWLMmDHppZN8fHw4ceIES5Ys4bHHHuOFF17A1dWVCRMmMHr0aG6//XYGDRrEyZMnmTNnDjVr1nTqrjz2xuRon/7+/kyfPh0fHx+8vLy45ZZb2LdvH6NGjWLgwIHUqVOH1NRUvvvuO0wmE/fcc0+OfT766KMsWrSIHj16MGjQII4dO8bcuXMzzJUE6NatG8HBwbRr147y5cvzzz//8Nlnn9G7d2+nvJZeeukl5s6dS9euXRk9enR6qaUqVapw9epVp/3bOPK3Om/ePJo1a5Zpfm+aPn36MHr0aHbv3k2LFi2cEp8QIh8Vwgp7IfLdv//+q0aMGKGqVaumXF1dlY+Pj2rXrp369NNP00sTKaVUfHy8euSRR5Sfn5/y8fFRgwYNUuHh4RlKLSUlJakXX3xRNW3aVPn4+CgvLy/VtGlT9cUXX2Q4Z2xsrLrvvvuUv7+/AjKU+wkLC1MPPfSQKlOmjHJ1dVWNGzfOVKYnrdTSu+++m+F4Wqmhm8v3pJXT2bFjR6b23bt3V35+fsrd3V3VrFlTDR8+XO3cuTO9zbBhw5SXl5fdv89Vq1aphx9+WNWrV095e3srV1dXVatWLTV69GgVFhaWqf1PP/2k2rdvr7y8vJSXl5eqV6+eGjlypDpy5EiGdp988omqWrWqcnNzU23atFF//PGHatmyperRo0eun39aqZ9Lly45HFPHjh2zLGM0bNiwTGW0li1bpho0aKDMZnN62aXjx4+rhx9+WNWsWVO5u7urwMBA1blzZ7V+/fqcf8HXvP/++6pSpUrKzc1NtWvXTu3cuTNTqaUZM2aoDh06qKCgIOXm5qZq1qypXnzxRRUVFZXpd3NzqaXevXtnOufN/StlLRl12223KTc3NxUSEqKmTJmiPvnkEwWoixcv5vgcsvv9Z8fW3+quXbsUoMaNG5dtHydPnlSA+t///mfXOYUQhUv2dhdCFBmGYVC2bFnuvvtuvvzyy8IOR9zg2WefZcaMGcTGxhb61p1CiOJN5nwKIQpFYmJipjmY3377LVevXs20naQoWAkJCRl+vnLlCt999x3t27eXxFMIkWcy8imEKBSbNm3if//7HwMHDiQoKIjdu3fz9ddfU79+fXbt2oWrq2thh1hqNWvWjE6dOlG/fn3CwsL4+uuvOX/+PBs2bKBDhw6FHZ4QopiTBUdCiEJRrVo1KleuzCeffMLVq1cJDAxk6NChvP3225J4FrJevXqxaNEiZs6ciaZptGjRgq+//loSTyGEU8jIpxBCCCFEMfP2228zZswYnnnmGT766KNs2y1cuJBx48Zx8uRJateuzdSpU+nVq1fBBZoFmfMphBBCCFGM7NixgxkzZtjckGPr1q3ce++9PPLII+zZs4d+/frRr18/h3YXyw8y8imEEEIIUUzExsbSokULvvjiCyZNmkSzZs2yHfkcPHgwcXFxLF++PP3YrbfeSrNmzZg+fXoBRZxZqZrzaRgG58+fx8fHx6lFrIUQQgiRf5RSxMTEULFixQzbFBeUxMTEDLuPOZNSKlNO4ubmhpubW5btR44cSe/evbnjjjty3LIXYNu2bTz33HMZjnXv3p2lS5fmKea8KlXJ5/nz57PdIUMIIYQQRduZM2cICQkp0HMmJiZSvao3F8Mt+dK/t7c3sbGxGY6NHz+eCRMmZGr7ww8/sHv3bnbs2GFX3xcvXqR8+fIZjpUvX56LFy/mOl5nKFXJZ9rWc2fOnMHX17eQoxFCCCGEPaKjo6lcubLTtiN2RHJyMhfDLZzaVQ1fH+eOukbHGFRteTJTXpLVqOeZM2d45plnWLduHe7u7k6No6CVquQzbVjb19dXkk8hhBCimCnMKXPePhrePs49v4H9ecmuXbsIDw+nRYsW6ccsFgtbtmzhs88+IykpKdMmEMHBwYSFhWU4FhYWRnBwsJOeQe6UquRTCCGEECI3LMrA4uQl2hZl2N22S5cuHDhwIMOxhx56iHr16vHyyy9nuftYaGgoGzZs4Nlnn00/tm7dOkJDQ3MdszNI8imEEEIIUcT5+PjQqFGjDMe8vLwICgpKPz506FAqVarElClTAHjmmWfo2LEj77//Pr179+aHH35g586dzJw5s8Djv5HU+RRCCCGEsMFA5cvNmU6fPs2FCxfSf27bti3z589n5syZNG3alEWLFrF06dJMSWxBK1V1PqOjo/Hz8yMqKkrmfAohhBDFRGF+fqed++KRKvmy4Ci47ulSl5fIZXchhBBCCBsMDOyfoWl/n6WRXHYXQgghhBAFRkY+hRBCCCFssCiFxckzFZ3dX3EhI59CCCGEEKLAyMinEEIIIYQN+bE63dn9FReSfAohhBBC2GCgsEjy6RRy2V0IIYQQQhQYGfkUJUZKcgoHfjtM9OVovPw8adyhAe6eboUdlhBCiBJALrs7jySfothLTkxm/luL+fmLNcRcjU0/7uHjTs+HuzB0wkC8/LwKMUIhhBBCpJHkUxRrSQlJvNJ9En9vPYIyMn6DTIhJZOlnq9i5di8fbnkT3yCfQopSCCFEcSellpxH5nyKYm36899yKIvEM41hMTj77wXeGf5ZAUcmhBBCiKxI8imKrZiIWFbP+hUjm8QzjWEx+HPFbs4dvVBAkQkhhChpjHy6lUaSfIpia8vCbVhSLHa11U0667/bks8RCSGEEMIWmfMpiq3L566im3W7ElBN07h87moBRCWEEKIksuRDnU9n91dcSPIpii1Xd9ds53pmxc3DNR+jEUIIUZJZlPXm7D5LI7nsLoqt5l0aYVjsmzFjSbXQtHOjfI5ICCGEELZI8imKrbqta1GzaVU0XcuxnaZBQHk/2vZpVUCRCSGEKGlkwZHzSPIpii1N0xj9+QhMJj37BFQDBTwz7TFMZlOBxieEEEKIzCT5FMVaw7Z1mbJ6LF5+ngDpSaimARq4ebgx9ofnaNevTSFGKYQQorgz0LA4+WaQ85W7kkoWHIlir1nnRvxwdgZbFm5n4w+/ExkejU+gF+363UKXB27Dy9ezsEMUQgghxDWSfIoSwc3Dja5DO9J1aMfCDkUIIUQJZCjrzdl9lkbF5rL7tGnTaNKkCb6+vvj6+hIaGsqqVasKOywhhBBCCOGAYjPyGRISwttvv03t2rVRSvHNN9/Qt29f9uzZQ8OGDQs7PCGEEEKUYGnzNJ3dZ2lUbJLPu+66K8PPkydPZtq0aWzfvl2STyGEEELkK0k+nafYJJ83slgsLFy4kLi4OEJDQ7Ntl5SURFJSUvrP0dHRBRGeEEIIIYTIRrFKPg8cOEBoaCiJiYl4e3uzZMkSGjRokG37KVOmMHHixAKMUAghhBAlkaE0DOXckUpn91dcFJsFRwB169Zl7969/Pnnnzz55JMMGzaMQ4cOZdt+zJgxREVFpd/OnDlTgNEKIYQQQoibFauRT1dXV2rVqgVAy5Yt2bFjBx9//DEzZszIsr2bmxtubm4FGaIQQgghSiCZ8+k8xWrk82aGYWSY0ymEEEIIIYq2YjPyOWbMGHr27EmVKlWIiYlh/vz5bNq0iTVr1hR2aEIIIYQo4SzoWJw8Zmdxam/FR7FJPsPDwxk6dCgXLlzAz8+PJk2asGbNGrp27VrYoQkhRI7+OxHO4tV7+WPnURISUwjw9aRH54b0uaMJZQK9Czs8IYQoUMUm+fz6668LOwQhhHCIUoppc7cwf+kOTLqG5dpeegmJUcxZuI3vfvqT8c/2plNonUKOVAhhi8qH1e5KVrsLIYRwpjkLtzF/6Q6A9MQzjWEoUi0WXv/gF3YdOF0Y4QkhHJC24MjZt9JIkk8hhMgHUTEJfLNoe45tlLKOjn7+7eYCikoIIQpfsbnsLoQQxcnKjQexGIbNdkrBv8fDOHzsIvVqBhdAZEKI3LAoHYty8oIjZbtNSSQjn0IIkQ/++e8iOHBJ7fDRi/kXjBBCFCEy8imEEPkg1WIA9g1raBpYLLZHSYUQhcdAw3DymJ1h53tESSMjn0I4iVKKxPgkUlNSCzsUUQSEVPBH1+wb+VQKKgb7529AQghRRMjIpxB5FHbqEr9MW8PKL9cTExEHQM1m1eg3uhe339ceVzeXQo5QFIY7b2+cvtLdljIBXrRpWi1/AxJC5Ilsr+k8MvIpRB7sXLuPhxs8y8L3f0lPPAGO7z/F+498wf/ajyX6SkwhRigKS5VKgXRoUwtdt/3h8uA9t2IyyduxEKJ0kHc7IXLpxIFTvN73bVKSUjBumq+nrtV0PLr3JOP6vI1hx6pnUfKMfboXDWpVQNMyLz1KS0oH9m7B3T2aFXhsQgjHpK12d/atNCqdz1oIJ/j+7SVYLEZ6opkVw2JwaNu/7F5/oAAjE0WFp4crH08cxOjhnQku55fhvsZ1KzL5pb48/VBnNDvnhgohCo91wZHzb6WRzPkUIheir8SwZeE2jFTbI5q6WeeXaWto1a1pAUQmiho3VzOD7mzJgF4tOB8Wad3b3c9T9nQXQpRaknwKkQtnjpzHYkfiCWCkGhzdcyKfIxJFna5rhFQIKOwwhBC5ZKBjkVJLTiGX3YUoAKp0vr8IIYQQmcjIpxC5UKl2MLpJz7TQKCu6SadG06oFEJUQQoj8kj/ba5bOkQkZ+RQiF/zL+nHbPbdgMtv+EzIsBn2e7F4AUQkhhBBFnySfQuTSkFf6o2kaOS1U1s06dVrVpFV3WWwkhBDFmYGeLzdHTJs2jSZNmuDr64uvry+hoaGsWrUq2/Zz5sy59jl1/ebu7p7XX0WeSfIpRC7ValadCYtfxOxqRr+pQLh2rbBjtQaVmbxiDLouf2pCCCHyJiQkhLfffptdu3axc+dObr/9dvr27cvff/+d7WN8fX25cOFC+u3UqVMFGHHWZM6nEHlwS++WfHXwQ37+fDUrv95AQkwiAFXqV6Lf6F7c8WAH3D3dCjlKIYQQeWVRGhbl5O01r/UXHR2d4bibmxtubpk/O+66664MP0+ePJlp06axfft2GjZsmOU5NE0jODjYSRE7hySfQuRRxZrBPPHBcB57byhxUfGYXc14eBX+ZQ0hhBDOY8mHUkuWa6WWKleunOH4+PHjmTBhQs6PtVhYuHAhcXFxhIaGZtsuNjaWqlWrYhgGLVq04K233so2US0oknwK4SS6ruMTIIXDhRBCOObMmTP4+vqm/5zVqGeaAwcOEBoaSmJiIt7e3ixZsoQGDRpk2bZu3brMmjWLJk2aEBUVxXvvvUfbtm35+++/CQkJcfrzsJckn0IIIYQQNhhKx3ByqSXjWqmltAVE9qhbty579+4lKiqKRYsWMWzYMDZv3pxlAhoaGpphVLRt27bUr1+fGTNm8OabbzrnSeSCJJ9CCCGEEMWEq6srtWrVAqBly5bs2LGDjz/+mBkzZth8rIuLC82bN+fo0aP5HWaOZAmuEEIIIYQNaXM+nX3LK8MwSEpKsu85WCwcOHCAChUq5Pm8eSEjn0IIIYQQxcCYMWPo2bMnVapUISYmhvnz57Np0ybWrFkDwNChQ6lUqRJTpkwB4I033uDWW2+lVq1aREZG8u6773Lq1CkeffTRwnwaknwKIYQQQthigNNLLdneoDmj8PBwhg4dyoULF/Dz86NJkyasWbOGrl27AnD69OkMdaUjIiIYMWIEFy9eJCAggJYtW7J169ZsFygVFE2p0rOxaHR0NH5+fkRFRdk9sVcIIYQQhaswP7/Tzj1jd0s8vJ07ZpcQm8rjLXaVurxERj6FEEIIIWzIzXaY9vRZGknyKYQQQghhg0XpWJxcasnZ/RUXpfNZCyGEEEKIQiEjn0IIIYQQNhhoGDh7wZFz+ysuZORTCCGEEEIUGBn5FEIIIYSwQeZ8Ok/pfNZCCCGEEKJQyMinEEIIIYQNztoO8+Y+S6PS+ayFEEIIIUShkJFPIYQQQggbDKVhOHt7TSf3V1zIyKcQQgghhCgwMvIphBBCCGGDkQ9zPmV7TSGEEEIIkSVD6RhOLo3k7P6Ki9L5rIUQQgghRKGQkU8hhBBCCBssaFicvB2ms/srLmTkUwghhBBCFBgZ+RRCCCGEsEHmfDpP6XzWQgghhBCiUMjIpxBCCCGEDRacP0fT4tTeig8Z+RRCCCGEEAVGRj6FEEIIIWyQOZ/OI8mnEEIIIYQNFqVjcXKy6Oz+iovS+ayFEEIIIUShkJFPIYQQQggbFBqGkxccKSkyL4QQQgghRP6SkU8hhBBCCBtkzqfzlM5nLYQQQgghCoWMfAohhBBC2GAoDUM5d46ms/srLmTkUwghhBBCFBgZ+RRCCCGEsMGCjsXJY3bO7q+4kORTCCGEEMIGuezuPKUz5RZCCCGEEIVCRj6FEKKES7UY6JqGrpfOURYhnMFAx3DymJ2z+ysuJPkUQogS6HJELD+v28+ytfu4EhGHpkG9msEM6NWC29vWxcXFVNghCiFKKUk+hRCihNn79xlefGsxScmpGIYCQCk4ciyMNz9ZyaKVu3l/7D34+ngUcqRCFB8WpWFx8hxNZ/dXXEjyKfIsNSWVP5bu4J/t/2JJsRBcvRxdHrgN/7J+hR2aEKXOqbNXeGHyTyQnWzCUynBf2s9Hjofx0pQlfDHpXrkUL4QocJJ8ijxZP3cL05//hqhL0ZhcTGiAxWLw5ctz6fHI7Tz10UO4urkUdphClBrzlv5FSkrmxPNGhqE4eOQ8f+07ya3NqxdgdEIUX7La3XlK50xX4RS/TF/L1KGfEnUpGgBLioXUFAvKUFhSLaz8cj2v93mb1JTUQo5UiNIhJi6Rtb/9g8XIPvFMY9I1Fq/aUwBRCSFERpJ8ilwJP32Jz0Z9lWMbZSh2r9/P8hnrCiiqgmexWIi6HE1MRCwqh5EmIQrCmfMRpKYadrW1GIp/j4flc0RClBxK6RhOvilVOtMwuewucmX5jHWgaYCthEtjyScr6TuyB5pWci4vhJ+5zM+fr2bFzPXERsYBUK5KGfo81YNeI7rgE+BdyBGK0sjR7z/ydUkI+1nQsODkBUdO7q+4KJ0pt8izzQu3YVhsj7AopTh/9CKnD58rgKgKxsHf/+HRhv9j4fu/pCeeAOGnL/P1q/N4ovmLnD92sRAjFKVVSAV/THYuINJ1jRpVyuRzREIIkZkknyJX4m5IuvKjfVF14UQYr/Z6i8T4pCyTb2UoLp+7yktd3yAhLrEQIhSlmZ+PB51C69iVgBqGon/3ZvkflBAlhKGuLzpy3q2wn1XhkORT5IpPoGOXlR1tX1Qt+XglSQnJqBzeMQyLQdjJS2z8/o8CjEwIqwf632Jziovp2qhn21Y1CygqIYS4TpJPkSudBrdDN9l++WgaVK5XkZA6FQsgqvyVnJTC6lm/2jXdQNM1ln2+qgCiEiKj2tXLMenFvriY9UwjoBrWv8lKwf58MG4AZjv+hoUQVs5ebJR2K42KzbOeMmUKrVu3xsfHh3LlytGvXz+OHDlS2GGVWr1G3IE964cU0P/p3iVisdGVc1dJiLXvUroyFGdK0DxXUby0b12Tbz4cTv8ezXC/oc5uhfJ+jB7ema/eeZAyJeRqhBCi+Ck2q903b97MyJEjad26Nampqbz66qt069aNQ4cO4eXlVdjhlTplQ4J4dsYTvP/IF9m20TSNW+9qSa8RXQowsnzkaP5cAhJuUXxVqRjIs490YdSwTkTFJOJi1vHxdi8RXwSFKAwGGoaTV6c7u7/iotiMfK5evZrhw4fTsGFDmjZtypw5czh9+jS7du0q7NBKrR4PdWbcj89RJiQIAJPZhNnFhKZpuLi7cPezvXl94fOYTKZCjtQ5ylQKxMvP0662mq5RvVGVfI5ICNvMZhNBAV74+nhI4ilEMTdt2jSaNGmCr68vvr6+hIaGsmpVzlO8Fi5cSL169XB3d6dx48asXLmygKLNXrEZ+bxZVFQUAIGBgdm2SUpKIikpKf3n6OjofI+rtOkwIJR2/duwc80+/tn2L6kpqVSoUZ5Og9vi5VeyRqRdXF3oPeIOFn243Oa8T2Uo+o3qWUCRCSGEyG8WpWFx8naYjvYXEhLC22+/Te3atVFK8c0339C3b1/27NlDw4YNM7XfunUr9957L1OmTOHOO+9k/vz59OvXj927d9OoUSNnPQ2HaaoYbstiGAZ9+vQhMjKS33//Pdt2EyZMYOLEiZmOR0VF4evrm58hihLq0tkrPNb0eeKjE7JNQE1mnYq1KjB99zu4ursWcIRCCFHyREdH4+fnVyif32nnHrLhAVy9nfuenhybzA9d5ubpeQUGBvLuu+/yyCOPZLpv8ODBxMXFsXz58vRjt956K82aNWP69Om5jjuvis1l9xuNHDmSgwcP8sMPP+TYbsyYMURFRaXfzpw5U0ARipKqbEgQU9eOw8vPE+3mlcSaBhpUrBnM1LXjJPEUQghhl+jo6Ay3G6/aZsdisfDDDz8QFxdHaGholm22bdvGHXfckeFY9+7d2bZtm1Pizq1id9l91KhRLF++nC1bthASEpJjWzc3N9zc3AooMlFa1GlZk9mHP2bVVxv4+Ys1XDp7BYAqDULoP7ont99/Gx5e7oUcpRBCCGcysBaGd3afAJUrV85wfPz48UyYMCHLxxw4cIDQ0FASExPx9vZmyZIlNGjQIMu2Fy9epHz58hmOlS9fnosXC3cXvmKTfCqlGD16NEuWLGHTpk1Ur169sEMSpZhfGV+GvNKfIa/0JzUlFU3TMJlLxsIqIYQQBevMmTMZLrvnNHBWt25d9u7dS1RUFIsWLWLYsGFs3rw52wS0KCo2yefIkSOZP38+y5Ytw8fHJz1r9/Pzw8PDo5CjE6WZ2aXY/BkJIYTIJZUPpZbUtf7SVq/bw9XVlVq1agHQsmVLduzYwccff8yMGTMytQ0ODiYsLCzDsbCwMIKDg/MYed4Umzmf06ZNIyoqik6dOlGhQoX024IFCwo7NCGEEEKIQmEYRrZzRENDQ9mwYUOGY+vWrct2jmhBKTZDNsVwUb4oYtJeQ1LrUAghhKMMlQ9zPh3sb8yYMfTs2ZMqVaoQExPD/Pnz2bRpE2vWrAFg6NChVKpUiSlTpgDwzDPP0LFjR95//3169+7NDz/8wM6dO5k5c6ZTn4ejik3yKURuxMcksP67LSz7fBVnjpwHoHLdivQd2ZM7HuyAp49M2RBCCFE8hIeHM3ToUC5cuICfnx9NmjRhzZo1dO3aFYDTp0+j69cvardt25b58+czduxYXn31VWrXrs3SpUsLtcYnFNM6n7lVmHXCRME7f+wiL93xBmGnL6EBaa90TbPuOV++SlneWf86FWsW7twXIYQQOSsKdT77r3sIFy/nltBLiUtmSdfZpS4vKTZzPoVwRFxUHC92mWgtg6SuJ55w7b8VXD53hRe7TCQuKq7Q4hRCCFE8pF12d/atNJLkU5RIq2dt5NKZKzlug2lJNbh05gqrZ20swMiEEEKI0k2ST1HiKKVY+tkqFLZnlCiutS09s0+EEELkgnGt1JKzb6WRJJ+ixElKSObiiXDsyD1BwcUT4SQlJOd7XEIIIYSQ1e6iBFJG9pfanfkYIYQQpUdRKLVUUsjIpyhx3L3cCQj2t7t9QLA/7rIXuxBCCFEgJPkUJY6mafR5sju6bvsbpaZr9H2qhxSeF0IIkSNZ7e48knyKEqn3413x8vdCN2X/EtdNOt7+XvR67I4CjEwIIYQo3ST5FCVSQDk/pq4dh5evR5YJqG7S8fL1YOracQSU8yuECIUQQhQnMvLpPJJ8ihKrdosazDzwAUNe7odPoHf6cZ9Ab4a83I+ZBz6gdosahRihEEKI4kKST+eR1e6iRCtTMZCHJt3L0AmDiLocDYBfGV9MZlMhRyaEEEKUTpJ8ilLBZDYRGByQq8cqpYgMjyIlKQW/sr64ebg5OTohhBBFnQKnF4UvrdubSPIpSjSLxcKOVXtZM/tXLpwIx9XDleadG9H7sTsoW7kMMRGxWFINfAO9M42GJsQlsuqrDSz7bBXnj4UBYHY1c/u97en/TC9qNateGE9JCCGEKNYk+RQl1rmjF3it9xTO/XcB3aSn7/N+5K+jzJ+yGE8fD+KjEwDw9PWg16N30HdUD4KrlSPyUhQvdXmDk3+fydBnanIqG+ZtYf3cLbww6ym6PtixwJ+XEEKIgidF5p1HFhyJEuny+av877ZxXDhhHbFMSzzT/1uRnniC9b8Xf7yCRxs9x+4N+xl319ucOnwWpVSmfd8tqQaGxeDdhz5n3+a/C+YJCSGEECWEjHyKEmnem4uIvhKDkWr/tpmGxSA5MZmxvaeQkpxqs72maXz/1mKadmyYl1CFEEIUAzLy6Twy8ilKnLjoeNZ+swmLA4lnGmUoUlJS7drxyLAY7Fq3n4snw3MTphBCCFEqycinKHH+3XmM5MSU3HegQDmwBvHsvxcIrlYu9+cTohRTSrHn7zMs33CAM+cjcDGbaFK/En26NqFief/CDk+IdDLy6TySfIoSJzkhuUDPZ88e8kKIzC5HxPLylCUcORaGSdewGNYvfQf/Pc/cJX8xsHcLRg3rhCmHbXKFKCiSfDqPJJ+ixCkTElRg59JNOtUaVS6w8wEkJyaz+cdtbPlpG9GXY/AJ9KH93bfQeUhbqUEqio3o2ERGjf2BC+FRAOmJJ4Bx7b8XrthNSqqFFx7rWigxCiHyhySfosSp0aQqVRtW5vShs5lWqjuTbtJp169NrovX58bu9fuZNPhDYiJi0XQNZSg0XePPFbuY/twcXp3/LG16Ni+weITIrR9/2cn58Kj0RDM7S9fso88dTahTo3wBRSZE1pTSUE4eqXR2f8WFXMsQJY6maQx+sW+uE0/NpBFcrSx6Dpf6NF3DZNa5f+w9uQ3TYfu3HOLVXm8RGxUHWBdH3fj/8dEJjOvzNrvX7y+wmITIjdRUC0vW7LOZeAKYdI0la/bmf1BCiAIjyacoke54sAP9n+4FgB0L19Ppuo6ntwdvLh9Ds87WEko3J6G6ruPq5sKbP79CzabVnBVyjpRSfPj4DAzDSE82s2pzYzshiqqTZ68SFZNguyHWy/F/7jmZvwEJYQcDLV9upZEkn6JE0jSNJz8czvNfP0Wl2hWyaXTTj7qGh687b68ZS7UGlXlr1WtMXPISTTo2wOxiQtMgqGIgD4wbwJz/PqVl16b5/0SuOfDbP5w9cj7bxDONMhQXT4SzZ8OBAopMCMcl2VFHNy/thRBFm8z5FCWWpmn0eKgz3Yd34t+dxwg/cwVXNzPVG1dh9/oDLP10Jcf2nQKgTEggdz3RnZ6PdiGgnB8AJpOJtn1b07Zva8A6smhP/c/8sGfDAUxmE5ZUi822JrOJ3esPFGhyLIQjygR6OdS+XJB3PkUihP1ktbvzSPIpSjxN06jbuhZ1W9dKP9bj4dvp8fDtWCwWlKEwu9j+UyisxBOs5aPsPb2mFXy5KSEcUb6ML80ahLD/8Dm75n3e2aVxAUQlhCgoctldlGomk8muxLOwBVUMxGKxbx6nYSiCKhbcCnwhcmNIn9Y2E09d1/DydKW7bGErioC01e7OvpVGknwKUQx0HNzW7pFXZShuv699PkckRN60b12ThwaGAlkvCtR1DReziXfG3I23l9SvFaIkkeRTiGIgqEIAnQa3zbH8E1yrPXp3G8pVKVtAkQmRe48Macf4Z3tTtVLGjSE0Ddq1rMmMKffTtEFIIUUnREZpcz6dfSuNiv71RlHorlyI4PfFfxJ9OQYPH3duvbMlIXUqFnZYpc7TX4zg1KGznDhwGiOLS/C6SadyvUo8N/OJQohOiNzpelt97mhfjyPHwjgXFonZbKJezfKUL+Nb2KEJkYEUmXceST5FtqKvxPDpqK/Ysmg7ylDoZh3DYjDjhW9p1rkRz0wbIUloAfLy9eTDLW/w7YSFrPhyHQkxien3eXi70+Ph2xn2xmC8fD0LMUohHKdpGvVqBVOvVnBhhyKEKACSfIosRV+J4em2r3HheFj6KJsl5XqZn/1bDjHq1jF8/MdkqtaXy2IFxcPbg8ffG8qwNwazb+NBoq/G4u3vRbPbG+Hh5V7Y4QkhRIml8uEyuYx82skwDHQ987wzwzA4e/YsVapUcUpgonB9/sysDInnzQyLQUJMIm8O+oAv979fqGWISiN3Tzdu6d2ysMMQQgghHGb3gqPo6GgGDRqEl5cX5cuX5/XXX8diuT4SdunSJapXr54vQYqCFREWyeYft2abeKYxLAan/j7D338cLqDIbIu+GsPC93/hoXrP0NvzPvr6DWXsXVPYsXqPbDkphBAi1xSglJNvhf2kCondI5/jxo1j3759fPfdd0RGRjJp0iR2797N4sWLcXV1Baw7wIjib+uyHXbXlDSZTWxasJVG7evnc1S2/b31CK/d+Rbx0Qnp21AmJ6awY/Ve/lyxm9Y9m/P6wudx95SyLUIIIURhsXvkc+nSpcyYMYMBAwbw6KOPsnPnTi5dusRdd91FUlISULg7wAjnib4Si8lGSZ80Simir8bkc0S2nTlyjle6v0nCDYlnmrQR3F1r9/HWvR/JlyQhhBAOM9Dy5VYa2Z18Xrp0iapVq6b/XKZMGdavX09MTAy9evUiPj4+XwIUBc/T1wPDYl+Cpmkanj6Fv7r6+ylLSElKyXHHFMNisO2Xnfzz539OPXdCXCIHfvuHnWv3ceLgaUluhRBCiBzYfdm9SpUq/PPPPxnmdfr4+LB27Vq6detG//798yVAUfBu6d2Cz57+2q62llQLbfu2zpc4LKkWTGaTzXbRV2PY+P3vWFJtTxUwmXV+mbaGBrfWyXN8EeFRfP/WYlbN+pXE2Otlj6o3rsLAF/pwxwMd5GqAECJLyoiChCWolD2gUsAUguZxD5pL3cIOTWRD6nw6j93JZ7du3Zg9eza9evXKcNzb25s1a9bQtWtXpwcnCkdwtXK06dWCnav35rjoSNc1yoQE0ap7U6ed+58//2PZ56v47ac/SU5Ixs3DlQ4DQ+k7sgd1W9fK8jEnD54h9YYyUDmxpBoc2vpvnuMMP32JZ28bx5XzEZl+Ryf/PsM7wz7jyI6jjPz4YUlARbGQlJSCoRTubi7yms1HSimI+woV+zGQAmiAAZhQ8XNQru3R/D9A0/0LNU6RmaE0NCcni7LDkQ0TJ07k/PnzWd7n4+PDunXr2L17t9MCE4Xr6c8eZWSbV4iNiM1yRFHXNXSziTHznsmy9JajlFLMHvs9309Zgsmsp58zKSGZX+f/xrpvNzN0/CAeeH1Apg9GW6vyb+Zo+6xiHXvX21y9kDnxBNLnnC77bDXVG1Wh92PyxUwUTXHxSaz49SA/rdzDubBIAAL9vbi7RzP6dG1CoL9X4QZYEsV9jor95IYDadN0rn2BTt6GuvogBP6ApsvvX5RMdmcNAQEBNGzYMNv7fXx86Nixo1OCEoWvfNWyfLrtLWo1rwFYV7WbzDomF+tl8HJVy/LerxNo1K6eU863+KMVfD9lCUCmZDft528n/siTLV9i94YDGeZVVqwVjL1ztnWTTpUGlfIU655fD3LiwGnbl/k1WPDOMpkDKoqk82GRDH/+Gz6ds5Hz1xJPgKuRccxasJX7n5nN4aMXMzwm/EoM3y3+k6nT1vL+l+tZs/kQScmpBRx58aVST96UeGbFAqn/QfzsAolJ2M/pZZau3Uoj2eFIZKtCjfJ89ucU/t11jM0LthJ5ORpPbw9uvaslzbs0dsqIJ0BSQhLfTvzRrrbH9p7k5a5vUP+W2rzx88v4l/WjXOUytOralN0bDthVmzSvI5FrZv+aYXQ2WwouHA/j761HnJakC+EMCYnJPDtxIeGXY7L88DOUIjYuiWcnLuS7j4bj4+3Ou9PXse63f0C7Xtlkyeq9fPjVBkYN68SddzQu4GdR/Kj47wET6aOc2TJQ8fPA6wk0TT6mRckjr2phU52WNanTsma+9b9l4XbioxMcesyRncd46Y43+HjrZDy83Bkypj+71u3P8TEms05InYrc0rtFXsLlwvFwuxY3pQk/fRna5emUQjjV2i3/cD4sKsc2Sili45P43xuLcHdz4d/jYRjpVbGvZ6yx8Um8PW0NCUkpDMzj31aJl/QrthPPa4wrkPovuDTI15CE/WTBkfM4Z+hKiDw4tvdE+uV8exkWg5MHz7D6618BaNqxIc999SSarmEyZ35Z67pGuSpleWvVa5hMjp3rZq7uLg61d3GV73iiaFm8ei/2rik6efYKh49dtCaeOfhk9q9cDM85oS31VFz+theimJDkUxQ6peyespnJ0k9Xps+p7PFQZz7ZOpnbBtyaIQENrODP0AmD+XzH25SrXCbP8Tbt2NDuKQe6Sad+aN7LOon8ExEVx/L1B5i/9C+Wrd3HpSuFv2lCflJKcfLMZafPNdM0jWU2rj6UenpZHHq30/P+fiWcJ23k09m30sjhIRmTycSFCxcoV65chuNXrlyhXLlyGfZ7F8IeVRuEkJrq+OtGKcX5Y2FEXoomoJwfAPXa1Oa1+f8jbno8Vy9EYHY1U65KmTyPdt6o54gufPfmQpvtdJNO276tKVMx0GnnFs4TGR3PJ7M2suGPw1gMha5rGIZC1zRuu6UW/3ukC2UCvQs7zGLDMBSbtv/L4/ffVtihFFmaRz9UzNt2tNTBXBfNXN12UyGKIYdHPrNbuZuUlJS+x7sQjug0pB1u7rl/7aQkpWQ65uXrSeW6lahQvbxTE0+AMhUDuf+1e3Jso5t03DxdeWjSvU49t3COiKh4Hh8zn/XXEk8gfXcsQyl+/+soj740l/ASOAqqaRpVQ4LsvuzuiNi4pGzvU8qCUgmlu/qDx92AO7ZHPw00r4cKICDhCENp+XIrjewe+fzkE2t5CE3T+Oqrr/D2vj4iYLFY2LJlC/XqyYpe4ThPHw8GvtCHuW8ucvixZlczfmV88iGqnA2dMAjDYjB/ymJ0XU9fZa/pGspQ+AZ6M2nFq1Spl7eyTiJ/vDt9LRfDo7LdjtViKCKi4njz45V8+sbgAo4u/93dsznvzVjn9H79fD0y/KyUAUkbUfHfQfI2QIHmgXLvj+Z5P5pLbafHUJRpuh8EfIaKeBxrYflsFi56DAL3vgUZmrBDfpRGcrS/KVOmsHjxYg4fPoyHhwdt27Zl6tSp1K2b/c5Yc+bM4aGHMn6ZcXNzIzExMZtH5D+7k88PP/wQsI58Tp8+PcNokqurK9WqVWP69OnOj1CUCg+OH8jl81etC4g0blxMmy2TWef2e9vj5uGW7/HdTNM0Hpp0L90f6syKGevYsWYviXFJlKtShu7DO9Nh4K2FEpewLexyNL/tOGrzTd9iKPb8fYYTZy5T3cZc4ZQUCzv2nyT8SixuLiaaNaxMhWtTQYqi7h3qM2/Jn4Rdjsk2AXeUpml0u61++s9KJaMi/wdJ67CWF7p2HpUACQtQCT+A75tongOdcv6iSBnRkLAUlfQ7EA96MJpHXwj4FmLfg5S0jVmuvenpZdC8RoDncNllSmRp8+bNjBw5ktatW5Oamsqrr75Kt27dOHToEF5e2W9K4Ovry5EjR9J/LuzXl93J54kTJwDo3LkzixcvJiAgIN+CEqWPrus8N/MJ2vZpzY/vLuPg74dtPsYwFP2f7mWzXX6qWDOYEe88yIh3Hkw/lhCbwLpvt/Dnil3ERccTVCGAzkPac0vvFnbtVS/y18at/6Khoez4hmPSNTb8fphH722f5f0Wi8G8pX+x4JddRMVkLBd2a4vqjBza0WbiWhg83F35aPwgnh6/gLDLeZ9aoGH9Xd3Z5XqtTxU1HpI2XPvp5jnd1p9V9FhrwuXeOc8xFDUqfgEqehKQnHYEMKESfwZTNbSA6aBSIW1vd3MIuLaXup5FmHXk09mllhxrv3r16gw/z5kzh3LlyrFr1y46dOiQ7eM0TSM4ODg3IeYLh1/lGzduzI84hEDTNELvakXoXa3Y+vMO3hz0AYbFyFQ4XjfpKEPx4qyR1GpetCbkb/zhDz4YMY3E+CRrgqMUukln04KtlK0cxBtLXy5yMZc2kdHx1sVFFjve9TWNiGxq0FosBuM/+IVN2//L8v6/9p5k36GzfPrGYOrVLDpv+mkqBfvzzQfDeWnKYvb/cy7X/aQNoIx9ulf6dpwq9TQkLsaeSxgq9qMSl3xaE89xWdxzLQm3nEFdGYIWtBjNs+RN6xCOi46OzvCzm5sbbm62r55FRVnLmwUG5rywNTY2lqpVq2IYBi1atOCtt97KcdfK/ObwgiOLxcLXX3/Nfffdxx133MHtt9+e4SaEM7Tt05pPt71F2z6t0PSM3zSbdmrIuxvG03Vo/mznevLvM2xdtoM/V+ziyoUIux+3eeE23rrvIxLjk0BdX5yXljxfOR/Bcx1f59ShM/kSt7CPh7urzZqVN/LMpq7rwhW7s008wToyn5ScystvLSElpWhWAfH2cuON5+8i0N8T3caATt0a5XF3s/4uTCYd07UHVCzvz9Qx/bmj/fU5/yrhR+z7eFGQ+g8q5WAun0HRo4zoayOeObGAikHFvFcgMQnnyM9SS5UrV8bPzy/9NmXKFJvxGIbBs88+S7t27WjUqFG27erWrcusWbNYtmwZc+fOxTAM2rZty9mzZ532u3GUwyOfzzzzDHPmzKF37940atSo0OcNiJKrVvPqjP/pRS6fv8qpv89gGIqQOhWoUL18vpxv67IdzJv8E//uPJZ+TNM12vVrw9AJg6jeqEq2j01OTObDx6fnOF/VsBgkJSTzxbOzmbr2dSdHL+zVtmUNvvz+d7vaWiwGbVtl3t3LYjFYsHynzccbhuJKZBxb/vqPLkV0i9UyAd5Mm3wfz0/6ibMXItJLTgHp/z3ozpaMHNqRpORUNm//l/PhUbiYTTSqU5HmjSpn/hxIOYzdO/kApBwBl+w/PIuVhKVcv9SeEwskrUFZLqOZit7UDFGwzpw5g6+vb/rP9ox6jhw5koMHD/L77zm/n4WGhhIaGpr+c9u2balfvz4zZszgzTffzH3QeeBw8vnDDz/w448/0qtX4c61E6VHmYqB+V4r88d3l/Hly3PRbxr+UYZi67Id7Fi1h7fXjKVR+/pZPn7zwm3ERcbbPI9hMdi9/gDnjl6gUq0KToldOKZ29XI0qF3BumtPDottdE2jUgV/mjUIyXTfwX/Pc+lKrF3n03WN1Zv+LrLJJ1gvwc/7+CG27T7OL+sPcD4sEhcXEy0aVaFft6aEVLDO8ff0cKVn5/xIEktO+SXr4iJ7WSD5L/CQz9PiQOH8V2paf76+vhmST1tGjRrF8uXL2bJlCyEhmd+jcuLi4kLz5s05evSoQ49zJoeTT1dXV2rVqpUfsYgSKjUllSM7jhEXFY9PoDd1WtVweu3NvNi9fj9fvjwXIMtkxLAYJCelMPaut5l74gu8/TOvKNy9fj+6Sc80PzVLGuzZcFCSz0L0ylPdeHzMfJKSU7P8N9d1DZOu89ronlle3bkaaf+2h4ahuBxR9LdJNJl02reuRfvWTnh/d6kDyX9g9+inuSSVXIrHoRRFFV65G1H8KKUYPXo0S5YsYdOmTVSv7vgaAovFwoEDBwp1ENHh5PP555/n448/5rPPPpNL7iJHifFJLHz3Z5Z9sZqoS9cnU5epFEi/0b24+9leuLg6tk96flj4/i82E0dlKOKjE1j7zSbufqZ3pvuT4pNQdpas0XWdpPjsi3GL/FejSlm+mHwv4977hbMXIjCZdOvuRrqGxWJQNtCbic/dRaM6FdMfc/rcVdb99g9XIuOIjLI9yn2j7OaNllSaxyBU3Ff2tLQmni5N8j2mAqMHYy0tZWfibSqbn9EIJ8qP7TAd7W/kyJHMnz+fZcuW4ePjw8WLFwHw8/PDw8NaZ3fo0KFUqlQpfd7oG2+8wa233kqtWrWIjIzk3Xff5dSpUzz66KNOfS6OcDj5/P3339m4cSOrVq2iYcOGuLhkfFNdvHix04ITxVd8TAIv3fEG/+46likpu3zuKl+/Oo/d6/fx5i9jcHUrvA/miLBIdq7da9dAhUKx6qsNWSafgcEB6CYdix3bhBoWg8AKeStVZrFY0HVdvgDmQe1q5fj+04fZdeA0m7b9S1RMAt5e7nS4pRa3NKuePgXjSkQckz5dyY59p9B1DU3THNqlR9M0bm1RI7+eRpGkmauh3O+CxBVkW0gdAIXm/XSJeh1rHn2s5ZTsoQeC6635G5Bwnvy87m6nadOmAdCpU6cMx2fPns3w4cMBOH36NLp+fcFfREQEI0aM4OLFiwQEBNCyZUu2bt1KgwYN8hJ5njicfPr7+9O/f//8iEWUIB8+PoP/dh/PdjRQGYo9vx5k5ovfMuqTRwo4uuuunI+w/49fwalDZ3mp6xv0HnEH7fq3wexi/RO648EOLPt8tY0OrNy93Lj1rpYOx3ru6AV+mbaWtd9sIuZqLGZXMy27NqFdvzbouk5qSirB1cvR7PZGRWpaQ1GmaRqtmlSlVZOqWd4fERXP46/O49K1WpjWS/SOfVrousadXUrIYhoHaH6TUUYkJP+GdeX7jUmoCTDQfMahuXcrlPjyjWt7MFUDyxlyHv3U0DyHoWmla1Rc5I09X3w3bdqU4ecPP/wwfaOgosLh5HP27Nn5EYcoQcLPXGbzgq02/0iUoVj55XqGvzEky3mUBcHFwcuhSin2bfqbPRsOUKFGeaasfo1KtSpQt3Ut6rauydE9J7CkZj/So+kadz3RDQ8vd4fO++v3v/POsE9R6nrpptTkVP5csZs/V+zO0DaoYgCDX+pHv2zmKwr7ffHdZsLzuAvQ08M7EeBXOK/vwqRp7hAwExJXWbfXTNlz7R4XcL8LzesBtJKywv0GmqZDwDTUlXtBxZB1AqqBW2fwGlHQ4Ym8yIfL7pTSvd0drvMJkJqayvr165kxYwYxMdYRgfPnzxMba9/qT1Gyrf9uS6banNlJTbaw+cet+RxR9kJqV8DfwW0Q05K/sNOXeL7TeCLCItE0jdcXPo9/OT90c9Z/Vpqu0aRDA4ZPuteh8+3ecIC3H/wES2rmgvtZuXI+gi+enc1HT8506PJwSXE5IpZf1u/n+593sOLXA0Q4OD8zTVRMAuu2/GN34qlfS/TT8n1XVzPPj7iDe3q1yNX5SwJNM6F53IketACt/D60ctvRyu9D93+7RCaeaTRzTbSgxeDWHeso7w30QDTvZ9H8P5PdjESp5fAr/9SpU/To0YPTp0+TlJRE165d8fHxYerUqSQlJcn+7oLw05etyacd8+11s0746cv5H1Q2TGYTfZ7sztw3Fzo8umWkGkSERbHo/V8Y8c6DlKtSls93TGXWq/P5df5vpN5QWNzb34s+T3Xn/nEDHJ7jOmfs9w61T7Ny5noahtal27BOuXp8cXM5IpaPZ21k8/Z/0xcPGYbCZNLp2r4+ox/qhJ+Ph939/bX3JKn2VC/AmnAGl/OlcsUA3FxdaNGoMj06NsTby3atvtJC0zxAs//3X9xp5hC0gI9QlkuQvMO6qt1UFlxvlUvtxZR1e03n91ka5arIfKtWrdi3bx9BQUHpx/v378+IEXIJQYCLmwMvK6VwKcQFRwD9n+nFhvm/ceFEGEYOl8yzYlgMVny5nmFvDMbV3ZWgCgG8OHskj783lL0bDxIfk0hAOV+a39EkVwurThw4xT9/Zr+LTk40XWPRB7/QdWjHEn/5/dKVGB57ZR5XI+PSv0Sk/b/FYrD2t0McPHKO6VPuw9/X064+Y+Psr0igFFSuGMD7Ywc4Hnw+UsoCSVsg9TCgwFwT3G6X5KcAaaayUsdTiJs4nHz+9ttvbN26FVdX1wzHq1Wrxrlzud8fWJQcTTo0YOmnq+xqa0k1aHxb1oXbC4q3vxfvbZzAuLve5uieE5jMeo7zNm8WFxXP6cPnqNXser013yAfOgwIzeFR9jm692SuH6sMxYkDpzl9+BxV6ztWhLi4efOTlVyNjMOSzei1YSjOh0fx3ox1THqxr119+vrYPy/XpGv4+9iX1BYUFb8YFfsBGOFcv/Rrsa6w9hoJng/k6kuJUoqd+0+zZPUedh04TVJyKi4uJiqW96dz2zr0797MoRFmIYqLolBqqaRwOPk0DAOLJfP11LNnz+Lj4+OUoETxFtqnFf7l/IgMj8qxnaZpVKodTJOOhVfuIU2ZioF8sXMqe349yMov1/HbT3/aVzD+mtTk1HyJy97aoTmJuBhZopPPE2cus/vgGZvtDEOx5c+jhF+JoVyQ7feqW5tXx9XFTHKK7X9bi6Ho3LaOXfEWBBX3FSrmnRuO3PCebVxFxbwJRhiazwsO9ZuUlMLrHyznjxu2oAVItRgcO3WJY6cuMfvHrYy4tz3392uTZXKblJRCRHQCri4mAvw8S/yovBAiM4eTz27duvHRRx8xc+ZMwJpAxMbGMn78eNlyUwBgdjEz6tNHmDT4g2zbaBqgwahPHykyHz6aptGiS2NadGnMqFte4d9d2ZeKyvhAKFs5f/ZmrlK/Up77cPMs2fMON/x+GJOuZTvqeSMFbNx6hMF3tcq2zelzV1myZi+7DpzGxayTnJJzn7quEeTvRWgRqeWpUg7dlHhmI24myrUdmpt9I/RKKSZ+tIJtu47n2M5iUUyf+xvJKRYeHtQ2/fi/x8P4cflu1v/+T/pc2krl/RnQuwV3dmmEh7trdl0KUTQozfmr00vpyKfDq93ff/99/vjjDxo0aEBiYiL33Xdf+iX3qVOn5keMohjqODCUl74ZhdnFlHHlu2ZN8lw9XJnw04u07Nq08ILMQa8RXe1KPHWTTusezQnKY9H47NRtXYuqDSvnOkH3CfSmZrNqzg2qiImITrD792PStWxXv1ssBh99vYH7np7F4lV7OH76MnEJyTn2p+saZrOJSS/2xWTKVfEQp1Nx35FphXWWTNYSSHY69N9Ftvx1FMPOFRKzFmzl7IUIAFZtPMijL81l3W+HMiziOh8eySezf+WJV+fnuiqBEAUlbcGRs2+lkcMjnyEhIezbt48ffviB/fv3ExsbyyOPPML999+fvrWTEABdH+xI6x7NWDN7E1sWbSM2Ig6/Mj50vrc9XYd2LLTanvbofG875oz7nqjLMTlefjcMg0Ev9Mm3OE7FnybkyTKcGmX7svLNdJPOnY93LdQdpAqCp7uL3WXfDaXw9Mh6hO3TORtZtNJaizKnUVSTSUcphWEoalcvx0tPdKNujfKOhp0vlFLXdhWyZ2tHCyT9ilKJ1pqcNixevcfuEWawXt1YunYfbVvW4K3PV1s/ZG96aNoH78kzV3h5yhJmTLmvyFwJEULkn1wVGTObzTzwwAPOjkWUQP5l/Rj8Ul8Gv2TfIo+iwsPLnSmrx/LC7RNIiEnItAApbS/4kR8/TLPOzq9XqJRi0dnFLL+wEr2Vju+zbkR/lGT3ltG6SadizfIMsnNxTXEW2rIG3/+80662hqGyvDx+6uyV9MQzJ5oGNaqUoXNoHW5pXr3IJJ3XpQCJmY4qBRZDw2y6OXE0wIgGk+3k8+CR83Ynnmnn/GPHMf47EW5zS1KLoTj03wV2HzxDy8ZV7D6HEAWqCGyvWVLkKvn877//2LhxI+Hh4RhGxg/l119/3SmBCVHYajatxrRd7/DD20tZ9+0mkhOvT/5r2qkhQ17uR4s7muTLuVdeXM3yCysBMDDwHuKGa0MTsQuSSdyYmjEB1Uh/A0tbqd8gtA6vL3qhSI8uO0vzhpUJqRDA+bDIHGu16rpGvZrB1K5eLtN9S9fus2tUTyk4dfYqd/doXkRreLpcu6WQatHYsrc6P21uyN/Hy2MonQCfeO5sd5i+7f+hXECc9SGat109WxxYgJcmNj6JMwci7Gpr0jWWrt0nyacQpYDDyeeXX37Jk08+SZkyZQgODs5wiUTTtHxNPrds2cK7777Lrl27uHDhAkuWLKFfv375dj4hgquV49npjzHinQc4ceA0llQLFaqXo1yVsvl2zgRLAkvPLct03LWxmcDGZoxEhYpWaG4ageYA2u7qwN+/HyY1OZXyVcvS4+Hbqdu6Vr7FV9RomsbY0T0Z/foPoMhyTqKua7i6mHnpiaz3Ed914LTdo3rJKan8ezyMFkUwSdI0DeV2O3GRm3ll+h3sO1oRXTMwlHU+akSMJ/PWNGPBhia89dg62jSrhKbbVyKqaqVA61ajDkxS8/JwtXsup8VQnCjEDSeEsEVKLTmPw8nnpEmTmDx5Mi+//HJ+xJOjuLg4mjZtysMPP8zdd99d4OcXpZeXryeN2tUrkHNtu/InyUb2S6x1dw3crW9YkUTSaERtBr9Q8i+v56RR3Yp8PHEw4z/4hUtXYjGZdAzDQNd1LBaDCmX9mPRiH2pVy/pLgz3llG6UkmrPnMpC4vkAr7+nc+BYMEB64pnGUDopqTBmRjdmTKxHnUD7uu3TrSl/OlB3VtOgddOqnL0YafdjdDu35RVCFG8OJ58REREMHDgwP2KxqWfPnvTs2bNQzi2Es6QaqeyPOkhE8lVcdBfq+tShvPv1uYMn4k6io2Ng+zKnjs7JuJM09Cv8WqmFrUm9Siya9hjb95zgt7+OEhuXiI+3B7e3rUPLxlVzTGwqlvfnfFiU3Vusli/r66ywne7AsYrsOFw5xzZK6RiG4ptfYLKd36natapJlYoBnL0Qadfop7ubCwN6tWDZ2v12tTfpGvVrBdsXjBCFpZTO0XQ2h5PPgQMHsnbtWp544on8iMepkpKSSEq6vkVedHR0IUYjSjtDGfxyfgVrLq4jzhKX4b6Gvg24t8pgKnuGYCgHt/iUd8N0JpNOu1Y1adeqpkOPu7NLY/6yY1RP0zTq1ChHtZAgm20Ly9I19s1ftRgav/11lCsRcQQF2J4bbDbpfDBuAKNeX8DFS9m/l2qa9d9hysv9qBoSRPvWNfl95zGbib3FUPTv3sxmHEKI4s/h5LNWrVqMGzeO7du307hxY1xcMpZxefrpp50WXF5NmTKFiRMnFnYYQmAog8+PTmdnxK4s7/8n+jBvHJrMK/VepIJ7MMrOhNLAINi9qK24Ln46tKlF+TI+XLoam2OSpJTi/n5t8iUGpZIhcS0qfh6k/ms9aK6L5nkfuHdD0+wrwn74WJjd81cNQ3Hy7BW7kk+A4HJ+fP3ugyxasZsfftlJQmLm6SFNG4Tw1IMdaVC7AgAPDWrLtt0nUMqSbU1DXddo17Im9WTkUxRhMufTeTSVU/2LLFSvXj3b+zRN4/jxnHe/cBZN02wuOMpq5LNy5cpERUXh61t0L5uJkmfNxXXMP/1Djm10dLzMXoxvMJaX9o+x67K7l8mLT5p/gFnPVeEKcYOTZ68wctwPxMYmZkre0koFDR9wK4/e297p51aWi6irD4PlKNa9P9L+7a/9t7k2WsAsNJPtLxpDRn3F2QuRdp/7o/EDadWkqsMxp6ZaOHMhkr2HzpCYmIKPtzuN6lbMclT4r70nGTN1KSmplgzJva5rGIaiTbNqTH6xj+xyJLIVHR2Nn59foXx+p5278vTx6B62y5I5wkhI5MwTE0tdXuLwJ9aJEyfyI4584ebmhptbUSyHIkoTQxmsvrjWdjsMYlJj+C/2KLeVbc+WS7/ZHAG9s2IvSTydpFpIELPefZBvf/qTVRv/zrAIqU6NcjzQvw2dQ+s6/bzKiENdHQqWtI0EbvzSce2/U4+jrg6DoMU2V6fXrFqWC2FRdo9+Vq6Yu925zGYT1SsHUb2y7SkIbZpVY8Hnj7Js3T5+WX+AKxGxmE0mmjUI4Z5ezQltUaPI7BAlhMh/efrUShs0lR0phMjesdjjXE2+aldbDY0/Lm/l2TqjiUiOYH/UATS0DElo2mKkLuU60zO4e36FXSqVL+PLi4935akHO3DkeBgpKRbKl/XN3zmeCYvBcoqcVzJYwHIcEpeB5705dtevW1M2b//P5ml1XaNNs2qUL1Mwoy1lAr15ZHA7HhncDqWUfG6IYki7dnN2n6VPrr5qfvvttzRu3BgPDw88PDxo0qQJ331n/x7BuRUbG8vevXvZu3cvYB2F3bt3L6dPn873cwuRW1Ep9i90UygiUyJx0V14vOajdC3XhQDXjCNTdXxq83TtkTxY9X75AM8nXp5utGhUhVuaV8/XxFMphYqfa2drDRX3rc1WLRtXpUHtCpjsKFs07J5b7Ty3c8nrVojSzeGRzw8++IBx48YxatQo2rVrB8Dvv//OE088weXLl/nf//7n9CDT7Ny5k86dO6f//NxzzwEwbNgw5syZk2/nFSIv3E2OTf1w1V357uQ8tlz+nWQjOf24m+7GbWXaMajyANwc7FMUUSoBLPZOZVJgOYZSyTkuPtJ1jalj+vO/iQs5duoSaGRY6JOWlL7+bG8a16uUh+CFKGVke02ncTj5/PTTT5k2bRpDhw5NP9anTx8aNmzIhAkT8jX57NSpU477A4viIyI8itVf/8ru9ftJjE+kbOUydBvaidY9m2EymQo7PKexKAs7r+62u72GxqWkS5yMO5VpwVGSkcSG8I0cjzvBK/VelAS0RMhFsXqVCjZWvgf4eTJ9yn2s3HiQhSt2c+a8dYtLV1czPTs15J6ezalRpUxuAhZCiDxzOPm8cOECbdu2zXS8bdu2XLhwwSlBiZJLKcWiD5bz9Zh5GIaBurYo4t+dx/lt0XYq1izPpOVjqFy3+I/IKKX48vgstl3Zbv9jUMSmxmW70EihOBF3krmnvueRGsOdFKkoNJo3aP6gIu1sHwCah11N3d1cuLtHc/p3b0ZMbCIpqQa+3u64uJScL3dCFCgZ+XQah+d81qpVix9//DHT8QULFlC7dm2nBCVKrkUfLGfmi99iSbWkJ54AhsU6yhd26hLPth9H2KlLhRWi0+yL3O9Q4pnG1gp3heKPK1uJSYnJbWiiiNA0DTwHY99bsQ6e9zk8X1LTNHx9PAgK8JLEUwhRJDg88jlx4kQGDx7Mli1b0ud8/vHHH2zYsCHLpFSINBHhUXw9JufFFZZUg9ioOGaP+55XvrV/wwKlFHs2HGDZ56vZveEAKUkpBJb3p8fDt9PrsTsoU9HODaydaF3Yeru3ydTQaOLXmANRB+1qbyiDv67upEv5zjbbiqJN87wPFf8dqETI9t9eB80DzXNwQYYmhLiR0qw3Z/dZCjk88nnPPffw559/UqZMGZYuXcrSpUspU6YMf/31F/3798+PGEUJseqrDXbtnW2kGmxesJWoy/atEk9JTmHykA95udub/LliF4mxiVhSLFw6e4V5k35iWK1R/LnS/nmXzpBqpHIw+pBdiSRYRzNddBe72+uaTmRKZB4iFEWFZqqAFvAl4AZkNTJpAs0NLWAmmkl2ALKXUgqlElEOblcrRHaUyp9baZSrOp8tW7Zk7lx7y4MIYbV7/f4Ml9pzkppi4e8/jtC2b2ubbT96fCZbfrJe3rakZvygMQyDlCTFhP7v8MGWN6l/S8FMDblxlbq99kXus7utUgpXXXaDKSk019ZQZhkqbra17idpO7O5g+fdaJ4PoZkd34WoNFIph1Bx30Hicqy/RxPKrQOa5wPg2l7KPAlRBOQq+bRYLCxZsoR//vkHgAYNGtC3b1/MZtlppSiKj0kgPiYBb38v3D0Lb4V0YnyS7UY3SEqwncCdPnyOtd9syrGNUgrDUHw7fgFTVo91KIbccje5Y9JMWJR9q5k1NFJUqu2G1xgYNPCtn9vwRBGkmauh+U1E+bwMxnnrQb2izR2NxHUqbjYqZgrWEeS0vz0LJG1BJW0E93vAbxKaJnNfRS7IgiOncThb/Pvvv+nTpw8XL16kbl3rVnNTp06lbNmy/PLLLzRq1MjpQQrHGYbBb4u2s+TTlfz9xxHAWv/v1rta0f/pXjTrXPD/TuUqB/HfruPpi4tsKVPJ9jzNFTPWoZt0m30aFoOd6/Zx4XgYFWrY3h87r3RN55bA1my/8pfNS+k6us1FRjer4lGZmt418hKiKKI03RP0WoUdRrGjEn65lnhC5hJW135O/AllCkDzeakgQxOi2LJYLPzxxx80adIEf39/p/Xr8JzPRx99lIYNG3L27Fl2797N7t27OXPmDE2aNOGxxx5zWmAi91JTUnlz0AdMGvIh/9ywzZ5hKP5csYsXu0xkzrgfCrxmatehnexOPMtWDqJhO9v7aB/+6z+7+0TB0b0n7WvrBHeU72Lf4iEMh5PPYdUeyG1YQpQ4ShmomPftaxw3B2XYt92tEBmkLThy9q0IM5lMdOvWjYiICKf263DyuXfvXqZMmUJAwPUt/wICApg8eTJ79uxxanAid754djZ/LPkLIFNiljYnct7kn1gxc32BxtWmV3Mq1CiPbrL9shv4fB903XY7w3BsMYHdiaoT1PSuweDKA222axNoe17rjXzM3tTykZExIdIlb7s+VcEmCyQsyddwhChJGjVqxPHjx53ap8PJZ506dQgLC8t0PDw8nFq15AOxsF25EMGKmevtGtX87o2FWCy52GEll0wmE5OWj8Hb3wuTOfuX3h0PdKDvqB529Vm1fkiOfd2sct2Kdrd1hl4VevBYjUcJcrXuD65d+x9AkGsQj9V4hLKuju0d7qbLzkZCZJB6GPs/znRUyj/5GY0ooTSVP7eibtKkSbzwwgssX76cCxcuEB0dneGWGw7P+ZwyZQpPP/00EyZM4NZbbwVg+/btvPHGG0ydOjVDIL6+vrkKSuTe2jmb7K7dcPVCBDtX7+WW3i3tah9+5jIXjoVhMutUbVgZnwBvh+OrUq8SX+ycyuyx37NpwVYsqdeT37KVgxj4fB/6juph16gnQK/HurJmziab7TRdo1bz6tRoUvArhtuVCSU06BYORf/D2YRzAIR4VKKBb310TWfRmcUO9VfWrWx+hCmEKEKUUpCyE1L/AzQw1wOXZrJaXxS4Xr16Adat1G98/Sml0DQtV4NYDiefd955JwCDBg1KDyJtlO2uu+7Kc0Aib079cwY0DXuW0OkmnVOHztpMPvdt+pvvpyxm17r96cfMrma63NeeIWPuJqR2BYdiLF+1LK989zRPfDCMQ1v/JTE+iTKVAmnUvp7dSWea+rfUpmmnBhz47XCOl9SVoXjwdduXwPOLruk08mtII7+Gme7zMLlj37+YVRXPyk6NTdhPpR5FxS+AlEOAApd6aB6D0Vxsz08W+chci+wL9N9MoZmL9m58KmE5KvYjsJzOeIepFvg8h+Z+R6HEVeqV0tXuGzdudHqfDief+RGEcJ7cbL2Xk1Vfb+DDx2ag6RnbpSansn7uFrb8tJ131r1OvTaOv5n7l/Wzq45nTjRN4/VFL/Bytzc5tuekdeHODX/MJrOOJdXgqY8eIvSuVnk6V35pHtCMH8/+ZHf7VoH2jVQL51EqCRX1CiSuIEMZn5Q9qPi5KLeuaH7vSlmkwuLaHvTyYGSeEpaZBh5353tIuaXivkbFTAWyeG+2HENFPgW+b6B5Dinw2Eq9UrrDUceOHZ3ep8PJZ34EIZynZtNqbJj3m11tDYtBzWbVsr3/0PZ/+fCxGdadQiyZv55ZUg2S4pN5tedbfHvsM7z9vXIbdp74Bvrw4ZY3WfnlepZ9torzx6wfQLpJp23fNtz9bG8atatXKLHZo6JHRer51OXfmP9yXB2voVHBPZja3jK3uiApZUFFjIbkLdeO3HhF59p/J21ART4BAV+jaS4FHWKpp2km8H4GFf2qrZbgcS+aqWhOXVHJ+64lnpD1kJj1mIoeDy4t0FzqFFhsonT77bffmDFjBsePH2fhwoVUqlSJ7777jurVq9O+fXuH+8tVVfjExET2799PeHh4ptXGffr0yU2Xwkm6DevE12PmkWrYnvIQXL0czW7Pvt7novd/QTdpWFKzvy5gWAxiI+NY9+1m+j/dK1cxO4O7pxt3P9Ob/k/34sr5qyQlJONfzg8v3+IxEvVQ9WFM/HsSiZbELBNQHR2zbubxmiNkzldBS1oPyZtsNDIgebt1ZNSjXwEEJW6meQ4AI9x6uTpDkXmwLkYywK0Hmu+YQonPHir+GzLHnhUdFT8PzW9iAUQl0pXSy+4//fQTDz74IPfffz+7d+8mKcm6YUxUVBRvvfUWK1eudLhPh1e7r169mipVqnDrrbfSp08f+vXrl36Tvd0Ln2+QDwNfsO8LwMOT78t2jmVMRCx/LP0r03aV2Vkxc53dMeYnTdMoUymISrUqFJvEEyDYvTzjG7xG5WvzOU3o6OiYru31Xd69HGPrv0I1L9lisaCp+LnY91apW7d1LIKUUqjkHRiRz2Nc6opx6Q6MiFGopD9K1N7nmvdTaIHfg1t34IZdjFzboPl/hub/YZEdmVYqGRJXYzvxxNomYWmB12oWpdOkSZOYPn06X375JS4u1/9+2rVrx+7du3PVp8Mjn6NHj2bgwIG8/vrrlC+f/zvFCMcNf3MIMVdjWT5jXfqcxzQms47FYjDyo4fpPKRdtn1cPnfV7pqYSinCTl/Oc9ylXbBHMG80ep3jsSfYcXUnsamxeJo9aRHQnDretVEoDGWgaw5/ZxS5pJQFkv/CvuEJA1IPoIz4IjX3UxmxqMhRkLyVDKNqlnOopLXg0hQCZqDptncUKw4015Zori2tyZyKBc0TTXMv7LBsM2IA+7fYhQSse9cXg+dWUpTSkc8jR47QoUOHTMf9/PyIjIzMVZ8OJ59hYWE899xzkngWYbqu8/QXI+g0uB3LPl/Ntl92kpqcioePO13uu40+T3WneuOcR9BcXB17aZhdZK9kZ6nhXZ0a3tUBSLIksfXKdr49OY+zCWcBKONahi7lO9OhbHu8zY6XuxKOSMHxT4dE4HryqZQFUnaDcQU0T+tcPd2xfzelkiBpC1jCQHMD11vQzFXseFwKKuIx6/mBLOerphxEXR0OQQvQNA+H4irKNM0VtGKUUDv8u9cB1/yIRIgMgoODOXr0KNWqVctw/Pfff6dGjdxt8+xw8jlgwAA2bdpEzZo1c3VCUTA0TaNpp4Y07WQt7WNJtWAy258gBlcvh19ZX6Iu2S4gazLrNOnQINexiqxdTb7K1MPvcTExLL0wPcDl5Mv8eGYRKy+s5qV6z5eq0ksWi8G23SdYs/lvwi7H4OHuQptm1eh9eyP882WahRtoXqDi7GzvCpq1vrFSFoifg4qbc9MqbHeUx91oPk/bHG1UKhUV+wXEfwsqGtKLcmko13ZoPmPQXHKoNJG41lorMkcWSD1i3fXH8z7bT1HkC033RLk0h5R92C4bZQLXdmhyFaRgldKRzxEjRvDMM88wa9YsNE3j/PnzbNu2jRdeeIFx48blqk+Hk8/PPvuMgQMH8ttvv9G4ceMM1/8Bnn766VwFIvKXI4kngNnFTJ8nuzNv8k82L79bUg36jrRvRyJhn2QjmXcOv0944iWATHu/KxRxqXFMPfwekxtNxN/VvxCiLFgnzlzmpbeWcCE8Cl3XMAzr72T3wdN8Of93Rg7rxMDeLZx6Tk3TUB53Q/x8bM/FM4F7HzTNbE0aI5+GpA1k/nRJhIQFqKRNEPQDmik4y96sfYyCpI039HHD/ydvQ10dAIHz0FyyXjio4r8jfbGNDSruG+tK8AJa0KZUKhjhoFLBVK54XBrPZ5rng6goe7aptqB5PZDv8QgB8Morr2AYBl26dCE+Pp4OHTrg5ubGCy+8wOjRo3PVp8PJ5/fff8/atWtxd3dn06ZNGd6oNE2T5LME6fd0T9Z+s4nL565ku/BI1zXa9GpB8y6NCzi6km37lT+5kHgxxzYGBvGp8awP/5UBIUW3bqEznA+LZOTYH4iNt66yTEs8wbqhV6rF4ONZv6JpMKCXkxNQz/tR8fPtaGlcTwjiZmSTeKaxgBGGihgJQYuyTvjiZt2UeGbRh0pCRTwOZTdlWkijlAEpe7Gv+LoCywnr6KrmZ0f73FPGVYifZ/2dGleuHXVFefRH8xqGZi7FpcTce0LiL5C0iez/3TVw7w2uUvawwJXSOp+apvHaa6/x4osvcvToUWJjY2nQoAHe3rmf9uXwmP1rr73GxIkTiYqK4uTJk5w4cSL95uyN50Xh8g304f1NE6lY0zoyo5uuv1zS9lO/9a5WvPbD/6T8j5OtD/s1w6X27BgY/Bq2CYsq2buJTZ/7G3HxSRmSzqx8/s0momMSnHpuzVwDzS+t6HdWb5k6oKH5vonm0gClkq2X2m1eT7NA6gFI2Z/pHqVSUfH29GGAcclaDiqr/u3e9SftxCmOtc+2mwMYkWMwLvXEuNQdI+IpVNIWjJQTqMt9UbGf35B4AiRDwiLrfYm/5ty3sqBSDqOSd6FSj5eoFd+aZkLz/xTc+2N9vZmu/X/aa0+3jk77TZX3XFFgHn74YWJiYnB1daVBgwa0adMGb29v4uLiePjhh3PVp8Mjn8nJyQwePNjhbRBF8VS+alm+PPAB237ZyfIZ6zh75Dwms4n6obXp81QP6t9SW94EnUwpxdmEc5kutWcnzhJHVEoUga7FaHGFA65GxrF5+79YbCSeYB0BXbHxIPf2ydvOWTfTPPqAXgYV+zGk3HRZ1NwIzWc0mtu1kaik30BF2dmzCZWwGM21acbDybvAsLeChI5KWIbm3jNjzJoLSg+6KcnLiRvoeRv1VCoRFfkCJK0l48r606ik9YAL1oQ4q6TYmiyryNEQtDjTlqVKJULcd9apBMYNVwXMdcBzOHjcXSLmQGqaK5r/26jUUaiEhel7u2su9cBjYLbTNET+05T15uw+i7pvvvmGt99+Gx8fnwzHExIS+Pbbb5k1a5bDfTqcfA4bNowFCxbw6qu2dpIQJYXJbKJ9/1to3/+Wwg6l1HB0NKe4Df4oZVxbQGMGzSvHLzD7/jlrV+Jp7Rd27jvl9OQTQHNri+bWFpXyn3WBDoC5ljUpuJFxkesLg2yxgCWL6RV2J54ABljCs77LY5B1CoA9C1g87s5TDUylDFTkM5C0+dqRLFbWY2tkVQEGKm42mv/b148asaiIh68txrnp95r6n3Vno+St4PeudbejEkAzh6D5/K+wwxA3KmULjqKjo601gpUiJiYGd/fr87ItFgsrV66kXLlyuerb4eTTYrHwzjvvsGbNGpo0aZJpwdEHH3yQq0CEyE9Xk69yJekqZs1MRY8KuJncCjukbGmaRrB7ec4nXrCrvbvujp+Lbz5H5RzKcsFatD1+wbXkEzCFgOeD1lGdLEoQJSU5UvsQEhKdc+k4O5pLbchphTluZPWJkpBkZuPuGhw/bx2hrl4hgttbnsDDPYuFNpqDK/ezKd2keQ5Bxc0CkrOM6VorQEPzetCxc94s+fdrc1TzygKJv6CMsemvBxX16rXpCdlvOUnicjDXAO9RDp9RqWRI+sM6hUHzsJayMuXuQ1WIksLf3x9N09A0jTp1Mm/lqmkaEyfmbpcth5PPAwcO0Lx5cwAOHjyYKRAhipK/ow6x4sIq/o4+lH7MTXOjvm89vMxeaBr4ufjTNugWQjxDCjHSjLqUv53vTs2z2U5Hp1O5Dpj1XO2UW6BU8g5UxKOgksgwEmc5h4p5G+LnQuB3aKaKGR5XJtD+Se0mXaN8GR/bDfOTa5sMPyoF89Y15bvVLUhIMmM2WZ97qkXnk0VtebBPIA8OVhnfP11bYa3hmGzHCTU0t05Z32OqAAHTUBFPYB19vHlusAlQaP7v53mhj4qbi31bQ9ojBSznQK+LSj0DSWuwZ4hIxc0Cr0ftXjmvVArEzbCu9M8wVUJHuXVF83kJzVx6SpmJom/KlCksXryYw4cP4+HhQdu2bZk6dSp169bN8XELFy5k3LhxnDx5ktq1azN16lR69cp5S+yNGzeilOL222/np59+IjDw+tQuV1dXqlatSsWKFXPoIXsOf2Jt3OiMb7ZC5L+1F9cz7/T36DctEklSSeyN2geQft+KCyup71OPJ2qOKBJli9qVCWX5hZVEJUdludc7gIaGi+5C1/JdCjg6x6nUU6irj2LdkeXm53MtqbCctxY7L/MLFouZ46cvk5CYQtlAb4ICvLgSYbvWpsVQ9LhW27awaOYqKNd21r3esfDZT6Es3Hi9GkSq5fpl4YQkF2YujOFq3EaeebhzegKq6T4oj/6QsAjbyZwZPLKvdqC5tYegH1Gx06/NxUz7/Wvg1gHN6wk01+a5eq4ZpPxlR6wOuHb5XCUsxrrYxo6+VSwkbgCP3rabqhRrtYHkzWRObA1IWo9K/hOCvkczS11rUTRs3ryZkSNH0rp1a1JTU3n11Vfp1q0bhw4dwsvLK8vHbN26lXvvvZcpU6Zw5513Mn/+fPr168fu3btp1CjrMm0AHTta57GfOHGCKlWqOHWAUVN5WCp49qx1x5WQkKIzYpST6Oho/Pz8iIqKwte3eFymFLlzIOog7x350KHH6OgEuAYwoeE4fF0KefQMuJBwkbcPv0tUSlSmxUc6Oq66K8/XfZY6PjldAi4ajOgJ1kvtNhKI+EQXFvz+HEs2JBIZfX3VeplAby5fjc3xsSZdo2KwP/M+fhhd11BGPCSuRKUeBpR1ZM/9Lod3F8oNlfIv6upAdh0O5H+f2E6EAD6eMIiWja/vWqQsl1FX7rHWwszh96b5vonmOdi+uCyXIfUooMBc3amLV4yLDbE9p9NOmhdauW1omjtG5IvWS+p2JbYmNO9n0LyfsNlSxX6Oiv2EnEdUTWCqjFZmdYlYzFScFebnd9q5q06dhJ7VNJk8MBITOfXy2Fw/r0uXLlGuXDk2b96c5RaYAIMHDyYuLo7ly5enH7v11ltp1qwZ06dPt3mO1atX4+3tTfv27QH4/PPP+fLLL2nQoAGff/45AQEBDsft8F+TYRi88cYb1n+IqlWpWrUq/v7+vPnmmxiGg2U9hMgnK86vQrejVNGNDAwikiP48cyifIrKMRU8gpncaCL3hPTHz+X6KmRPkwc9KnTjrcZvFIvEU6kEiF+MreQhMtaNJ9/ryzdLr2ZIPAG7Ek8fb3feGXM3mgYq9kvUpbbWhSjx8yH+e1T0BFR4KEbMR9YFT/lIc6mDFvgdizc3w6TbPpdJ1/hp5e4MxzRTGbSgBWBO2z0sbcQ07W3bA833LbsTz/Q+3W5Fcwt1/qppp/Vnss7/Tbt07tAiKGVXe2s5rG+wqxyW5SQk/+FADEI4Ljo6OsMtKSnJrsdFRVmni9x4Sfxm27Zt44477shwrHv37mzbts2uc7z44otER1vn6B84cIDnnnuOXr16ceLECZ577jm7+riZw5fdX3vtNb7++mvefvtt2rVrB1j395wwYQKJiYlMnjw5V4EI4SyXky7zT8zhXD3WwGDble3cW2UQXuasL2EUJG8Xb+6q2JveFXoSlxqHgYG32RtTcVrRa7mAdb/znI3/6g5Ohflj2Ci67OXpSlz89bmQZpNOl3b1GHFfe4LL+mLEvANxX93wiBsXLCVB3Bco4xL4TsrXeeoWvRF/HAixWZsUrNMFft9xjFSLgfmGerqaKRiCFkHKPlTCEus2nZobmmvotVHcwn+NptE8BqFiP8Th2qIZmEDzRvN66Hq/Ls1QCfZ+ITTAxY5NBpK3gYq0OyaVsAzN7TY724sSKx+LzFeunHFu8fjx45kwYUKODzUMg2effZZ27drlePn84sWLlC9fPsOx8uXLc/FizhuZpDlx4gQNGli/BP/000/cddddvPXWW+zevdvmvNHsOJx8fvPNN3z11Vf06dMn/ViTJk2oVKkSTz31lCSfxZRSivjoeJQCLz/PYr147HKSvXUNs5aqUvk7+hBtAp1frie3dE3HpwhMBcgvR04Hsee/Sna1dXd1YfKLfbkaFY+7q5km9Sul7+uuknfdlHhmI2EhuN0B7p3zEnaOEhNT7Eo80xhKkZCYjI9Xxst6mqaBazM012ZOjtDJPAdC3HRQCdiXgN647ee19xvNFy1wtnWhVBr3OyHmLVDxNvrTwFwLXJrZPrXlkh3xpTe2Jv1C5KMzZ85kuOzu5ma7IsvIkSM5ePAgv//+e36GhqurK/Hx1r+/9evXM3ToUMA62po2Iuooh5PPq1evUq9evUzH69Wrx9WrV3MVhCg80VdiWDFzPT9PW83ls9Z/P/9yfvR5sjt3PtGVgPL+hRtgLjhjVDDBkkiqkcqeyL0ciz2ORRmUcy9LaNAteJvzf85giWKqaC1fo7LfeWjV9rqYdAOLYXsm0JXIOJRSdLutfqb7VLy9K65NqPhv0fIx+XR3d8mwB70tuq7h4e6ab/HkN00PhICZqKuPYJ37efO/gQ4o8JmIZvJHxX13rXySxTqv0vM+8OiPpvve1K8n+LyIis6ppMu1clE+r9r3xVnzcOSZgVZ0RphFIcrHOp++vr4OzfkcNWoUy5cvZ8uWLTbX3QQHBxMWlvELVFhYGMHB9k2Vad++Pc899xzt2rXjr7/+YsGCBQD8+++/uV7z4/Ccz6ZNm/LZZ59lOv7ZZ5/RtGnTLB4hiqpT/5xlRJPnmT3u+/TEEyAyPIq5by7kkYb/47/dxW/L1BDPSrjkoVg2wNn4szy79wU+OzqNtWHr2RD+K/NOfc8ze55j7qnvSTUcqz1ZmmmaO3gM5PqcxczOX/a1K/FMcy4s8w5CSilIXId9C1MskPyHdT5qPjGbdG5rUwuTbjsZMukat7WugkkvwhWn7aC5tkYL+gnce5Dp39u1DVrAHHSvIWjuPdCD5qEHH0APPoRedo11X3c96w9fzfN+NJ+Xub7lZIZ7ARc0/4/R3NrZF6hrmyz6yeF5ySV3AdeTT2ffHAlBKUaNGsWSJUv49ddfqV69us3HhIaGsmHDhgzH1q1bR2hoqF3n/OyzzzCbzSxatIhp06ZRqZL1KtWqVavo0aOHY0/gGodHPt955x169+7N+vXr0wPftm0bZ86cYeXKlbkKQhS82Mg4Xu76BpHhUagsRmYMQxEXFc/L3d7kq4MfEBjs+Gq2wuJh8qB92XZsDt+SbZminJg1M2vDru+VfeO+6anKwvqwDVxKusQztUehywpYu2iew60lc1Q8WV2SjU90wfoubN90DxdzVr/3FOyri3kDIxZMjoyCOWZArxZs3v6fzXYWQ3H3rZ+hwqeiPAdbky1T7urnFTbNpTaa/4coy9hrO0EZYKqa53qZmtcj4NYNlfADJK61llXSg9Dc+4DnAOvIq719mcqi3Lpfqx9q68uKK7j3zVPsQjjLyJEjmT9/PsuWLcPHxyd93qafnx8eHtb3sqFDh1KpUiWmTJkCwDPPPEPHjh15//336d27Nz/88AM7d+5k5syZdp2zSpUqGVbKp/nwQ8cqytzI4eSzY8eO/Pvvv3z++eccPmxd1HH33Xfz1FNP5brYqMhfCbEJbPtlFxEXI3H3cqNF1yb8seQvrl6IzHEbR8NiEBcZxy/T1jJsov0raouCOyv05K8rO0iwJDiUgGpopKqcRzUVir2R+9h86Tc6l+uY11BLBc0cAgGzUBGPgIrjxgTUYugcO+fYvvSN6mT1XuOCdXch+1aJggZ6/s6jbd6wMvf1bc38ZTtybDekyz6a17lgzb/jvrZOHwj4Cs21cOYdX7wUzeWrsbi6mKhWOQhXF8c3MdBMQWBq69S4NHNlNJ8XwefFvPfl+yLq8rZrO23lVMpqQoGU5xJFX1HY233atGkAdOrUKcPx2bNnM3z4cABOnz6Nrl//gt62bVvmz5/P2LFjefXVV6lduzZLly7NcZHSjU6fPp3j/VWqVMnx/qzkqc5ncVPa6nwmJSQx+7XvWT5zPUnxSdfnn2ng5u5KUmKyXUP+fmV9+fHClxlezMXBmfizvHfkAyJTotDQMtXKvJmGhq+LD9EpMXa1reBegbcav1GsF2cVNGW5BAkLUPHz0/cv3/r3rbzyRRO7+2hYpwIzptyf5X3214Q0gett6IH2ffPPC6UUP/yykzkLtxEXn3xtNXsqqRYdL/ckhvXczeAuB8j4MtKtq9qDfkEzO/7Gnlu/7zjG98v+Yt8/59KP+Xi7069bU4b0aYWfT/6NEhcGlXrCuvuT5QTX5wqn/UO4ofmOR/O8p/ACFOmKQp3PapMn50udz5OvvVak8xJd13P8nLNYHN9cwu6vs//99x+vv/46M2bMyPQLioqK4sknn2TSpEnUqFHD4SCE8yUlJPFytzf5Z9u/6Qse0hc+KEhKsP/yZNSlaGIj4vANKl6rrSt7hvBu07f568oONl3aQnhSOEqBoQxiLbFoaOiajkVZMGsm2pdpx/7IgzYTT7COfp5PPE940iXKu8se0PbSTGXBexSa9yiUSgJ0/ljyKybT31gs9o1QN2uQ/QR3zfMBVOIyO3qxoHkNtS/oPNI0jXv7tObuHs3ZvP1fjh37HZW0hRoVr9Kx+QncXLJ64zZAJaPiv0HzHefUeJRSkPoPGBGgeYNLAzTNhVk/bmXWgq3oN81RjYlNZN7Sv1j/+2E+nzSEcsXsfSAnmrk6lFkFydtQCUutBf01TzTXduDRT0Y8RUb5uOCoKNuzZ0+Gn1NSUtizZw8ffPBBrisc2Z18vvvuu1SuXDnLzNzPz4/KlSvz7rvvpg8Ji8L17fgfMySeeVZMB/dcdVfal21H+7IZFyKcjj/Dwai/STaS8Xfxo1VgS7zN3jy5a5RD/cen2ir/IrKjadZSInHxyVnOO86KroFvDqNvmmtT8B6Niv005448h4GrnYtTnMTN1Uy3Dg0wGoyF1EPYLkdkgYRFKJ8X7d6rPCdKWSB+Pir+G7DccBlND2TDgcHMWmBNgrN6zzAMRfjlaF6cvJjZ7w3NlKAWZ5qmg1s7+xcrCVHKZLWYvFWrVlSsWJF3332Xu+/Ofnvf7NidfG7evJm5c+dme/+gQYO47777HA5AOF9ifBK/zFjrtMQzoLwf3v4lq9RIFc/KVPHMvADC0+RJvMX+FdCeZk9nhlUq+fq4o+kaWGy/Xg0Fft45X/rVvEeDHoSK+ehaIfG0t7nUawXMnwSvRwtvukTqUewuxK4SwBIG5qp5OqVSqajIZyBpfeb7LFf5dmk4mhaIyqGAtsVQHDt1iV0HTtO6ad7iEaJYKqUjn9mpW7cuO3bkPJ89O3Ynn6dPn6ZcuewvL5YpU4YzZ87kKgjhXDvX7CUhxvaOMvbQdY0+T/UodvM9c+uWoFtYdWG1zUVKGhqVPCpSzq1sAUVWcnVpV4+la/bZ1dZs0mnfpqbNdtaakQMgaQMq5ca93bunj7gWH3lPklXsp9cSz8yfdEfPBnH8fJBd/Zh0jV/W76d106ooIxYSf0YlbgQVA3pZNI87wa0Lmub4AiUhRNF0cyF5pRQXLlxgwoQJ1K6duy2e7X6H8PPz49ixY1StmvU33qNHjxbZybKlTUQWNRBzQzfp+AR60/vxrk7przjoXK4jKy+sstlOoeha/g5ZbOQEzRqEUDUkkDPnI3Icrdd1jTva10/fzcgWTXMF955o7j2dFapzuNSFlAPYNfqpeeV5z3SlEiD+W7IbYjl/xf45nBZDcfZCBCphOSrqNSABa3KsAB2VtAb0chAwDc2lcZ7iFqKoKQqr3QuDv79/ps86pRSVK1fmhx9+yFWfdiefHTp04NNPP+X222/P8v5PPvmE226TQrxFgWduVqSmfX7c8LOrvwujf34Y/7Kl50tFWbcyDK32AN+c/C7bNhrQ3L85Hcq2L7jASjBN03jz+T48+dp8ErLZklLXNUKC/Xn64fzbkaigaJ4PoKLsKRVkAo/B1iQ6LxLXXytvlTUXk2O1cF1MMaioSVwfkU3797rWj3EZdeUBCPoRzaWuw+EKUWTl497uRdnGjRsz/KzrOmXLlqVWrVqYzbm7ymH3o8aMGUNoaCgDBgzgpZdeom5d65vK4cOHeeedd1izZg1bt27NVRDCuZp3aYRu1jFS7ftQ6XxvO/7efoTwE9bSN6byGt4D3fG8y4VZ2izWHVzL4zVHUNkzd9toFTe3l+uEh+7O92d+JColKn27TkNZMGtmupS/nYEh90iBeSeqUaUMM6fcz3sz17Pn7zPomoamaxiGga5p3N62Lv8bcQe+3s4tc1Io3HtC7HSwnCT7klC6ddW1M1bkW86R05ajdapcQtcMDGX79azrGk2q7b72U3ZDNgaQjIqehBaU/Zc4IUTx0LGj8+tZ2518Nm/enEWLFvHwww+zZMmSDPcFBQXx448/0qJFC6cHKBwXGBzAbffcym8/bc85AdXAy9eToZ8NZMqJd3CJ98PAQHPN+E3sXMJ53jz0FmMbjMlykU5JFFrmVtoEtWZv5D6OxZ7AUBbKuZfj1sA2ssgon1QNCeLTNwZz6uwVtu0+QVxCEgF+nnS8pQ5BASVnwZumuULgbNTV4WA5TubLDrp1YVTgLOfscqS5ktOqhjJ+CdzW9CS/7a+GYWOLU8NQ9Gm/146TWiDlT1TqcTSzlN8TJUQpWnD0888/2922T58+Dvfv0HjpnXfeyalTp1i9ejVHjx5FKUWdOnXo1q0bnp7ygVyUPPbOg+zb+DcxV2OwZJWAXssvn//qSeZenE98ajzKVaFlsbjBwCDFSGHGsS+Z1GhiqZnnaNJMtAxoQcsA+VJVkKqGBFE1xL4FMMWVZgqGMksgYZl1N6PUf6136MFonveCxyDrDkHO4NISW/NLH7lzJ38eqkxyCtmOgGpA/y4JVCprb3kxDZK2giSfQhQ7/fr1s6udpmn5W2Q+jYeHB/3793f4RKJg+QR6M/6n5/nkqa84ceA0JrMJi8WCrusYFgMffy/+9+WT1O5Zla8OfGmzPwODswnnOBZ7jFo+tQrgGeTdpbNXWPXVBo7sOIrFYlC5bkV6PdqF6o2lTIwofJrmAZ5D0DyHoFQqYOR9fmdWXJqAud61BDfrJLRahUg+fHoFL33Rk5h4NzQN0va+M+kaFkPRp1tTRg9cDsn2DtXo2L/VqRBFX2lacGQYjs0Fd5TUwyhh9vx6gMUfr+DPFbvTC3dXqV+JoIqBePp64OHtTqtuzbhtwK24urmw8sJqu7aeBNDR2RWxp8gnnxaLhZkvfseST1aiaRrGtZ1z9v56gKWfruKW3i0YM+8ZvOxcNS1EfsvP0kSapoHvWNTVYWS+xH9dw+rhLPqoFut21uWX9QcIvxKDq4uJNs2qc3ePZtSuXg4jegck23vlwwJ60S9FppSy7nAUPxeS/wSVAqYKaJ6DwONuND2gsEMUolD8+uuvjBo1iu3bt2e5s2Xbtm2ZPn16rhabS/JZgsx5/QfmTfoJk1nPsGPM2SPnOf3POW69qyX/m/kErm4u6ffFp8anbzFpi6ZpxFmK9o4+Sik+fvJLVn+9AaXIkFSnTT/YsXovr3R/k/c3TsTVPR9Gmoooi7JwIeECSUYy/i7+BLkFFnZIooBorm0gYBoq4hkgrQZw2t+G9TK75vMaXl6D6dcd+nVvlnU/Hn1R8XPsPKsHuHXJfdAFQKkkVORzkLSODIuyLCdQMe9A7OcQMAPNtXVhhimKilI05xPgo48+YsSIEdnubPn444/zwQcfSPJZmq38cj3zJv0EkGmOZ1rpmj9X7Oaz0V/z3Mwn0u/zMnthKPuG15VSeJuL9sKPv7ceYdVXG3JsY1gMjvx1lJVfbqDf6CJWAzIfJFgSWHtxPRvCNxKVcr0GbG3vWvSs0F3mtJYSmlsnKPc7JCxBJf4Mliug+6C5d702x7S87T5cGqJcmkPKfrJfqQ+gW6cU6EX7/UJFvgxJae8XNz8fBSoedfURCPoJzSV3xbSFKK727dvH1KlTs72/W7duvPfee7nqW2rFFAPJRgoHo/7mzys7OBB1kGQjOcP9FouFbyf+aLMfZShWz/qVS2evpB9rFdjCrkvuYJ332TqwlWPBF7Cfv1iNyWz7Za2ApZ+tsl5yK8FiUmJ489BbLDm3LEPiCXA09hif/Pc5C84sLPG/B2Gl6T5oXkPRgxahl9uIXuZnNO/RdiWe6X34f2wtJI8pmxY6uLRG83neKTHnF5VyCJJWkvNiLANIQcV9UUBRiSJNXZ/36axbUR75DAsLw8XFJdv7zWYzly5dylXfdo183ry1Uk5klyPnSbIk8fP55fwavon4Gy53e5g86Fy2I30r3YW7yZ1da/dz5XyEXX1qmsbqWb/y4OsDASjrVpYmfo05GPV3jltK6uhU8axMda9qeXpO+W3Hqj1Zr+6/mYJz/13g0tkrlKtcJv8DKySfHv2CCwkXs/yCkXZs5YXVVHAPpkNZ2SRC2KaZgiHoJ1Tsh5CwDLjhy7DmB54PoHk/mT+Lp5xIxc8np/qn11kgcTXKMtZ5FQhE8VTKLrtXqlSJgwcPUqtW1us89u/fT4UKFXLVt13JZ1ZbK91MKZXrJfcis0RLIm8ffpeTcacyJQ4JlgRWHF3N7qN7GdPmRU7/c9a6it3O1WlnjpzL8PND1Ycx8e9JRKdEZ5mA6ui4m9x5suZjuX9CBSQpIdl2oxvbx5fc1bjHY09wJOZfu9r+fH4Ft5VpX2rKaIm80Uxl0Pwmo3xeurZIJw70IHC9tcgnnelS9mA78UxjgdTDYGqXnxEJUaT06tWLcePG0aNHD9zdM27wkZCQwPjx47nzzjtz1bddyefNWyuJ/PftybmcijudIfFUqYqEdSnE/ZhMyj8GF4hmkOkxajSu6tBlU13PeFk60DWA8Q3HMuvENxyIOoDlgsJyXKGUwlRdo2G9OjxcfTjB7vZfmiss/mX9MkwryJEGfiV469DNl35DR89xRDvNpaRL/Bv7H3V96hRAZKKk0HQ/cO9W2GHkjh2LLDOSgZVSr5SNfI4dO5bFixdTp04dRo0alWFny88//xyLxcJrr72Wq77tSj7zY2slkb3I5Ci2XfkzQ9KgEhVXX4on6S9Lhpm6yqI4vv+U3cmnUoqaTatlOh7oGkCviF5cGB/B/vWHMvxBVOycSNjrlwnuWPSTz27DO/H9lCXp5ZWyYzLrtOzWFN9AnwKKrOCFJYbZlXimCU8Ml+RTlB7mGmA5hd1JpalafkYjRJFTvnx5tm7dypNPPsmYMWPS8wxN0+jevTuff/455cvnLi/I9Wr3+Ph4Tp8+TXJyxsucTZo0yW2X4pq/rv6V6VJ7xFsJJO289iZ5Uz5xY1klW0wmnW7DO2U6/sfSv3hz0AfWF9dN3e3fcogXu0zk5W9G0+X+oj0vsPdjXVnwzjKUoXJMyC2pBnc/07sAIyt4Jj27BSHZtM/HWpOi8CiVCJjQtOwXDuTfuRWk7AbLGcAMLk3QzFUKPI6saJ5DUEnr7Wipg0urIhO3KDylqch8mqpVq7Jy5UoiIiLSd7asXbs2AQF5q3/r8KfNpUuXeOihh1i1alWW98ucz7y7mhyRofZm6mkLiWtTndL3gOf74Fcm46XmC8fDmDTkQ+u/XRZ/CGmjiO8M/4waTatSvVHRfRMuGxLEq/OfZdLgD6yVUm5OzK/V2B46YRAtuzYtlBgLSi3vmvwddcjuagY1vKvnc0SioChLOCr+B0j4HgzrNBRlqo3m9SB49LXurpSf51cKEhaj4qaB5XTG+1zbofk8j+bSKJd9J0PiWlTCYrBcBM0d3DqgeQ5GMzmw+MG1/bWdn/4j59FPheb9ZK5iFaKkCAgIoHVr59W7dbjU0rPPPktkZCR//vknHh4erF69mm+++YbatWs7tBG9yJ6r7pph1C7+55Tsq5pkQTdl/GdNKz105+NdeWjSkEztf5m2xppg2shRNA2WfZr1l46i5La7b+Gd9a9T/5bMdfkq1arAy9+OTl/tX5J1KtvBrnY6Og186xeLOb0Xw6OYMe83HnrhW+4d9TXPTPiRNZsPkZTsnC9nJYFK3oW63APivkhPPAGwHEVFv466cg/KEp6/McS+h4oec23E8ybJ21BXhqCStjneb8o/qEu3o6Keg+StYDkKqQchbjrqUidU7Od2T0HSNB0t4EswVSbrj0IToKH5TkBzk4VGQjiTwyOfv/76K8uWLaNVq1bouk7VqlXp2rUrvr6+TJkyhd69S/alzIJQ37cey87/kv5zygmLQ3Pdm9xWn//2nCAuKh5Xd1dC+7Tkrie706RDgyxXM6/6+lebcyTBeql63XebGfXZI5hdsn/p/Lf7OGtmbyTs1CVc3F1o3L4+XYd2xNu/4ApON+3YkI//mMyJg6f5b9dxDItBSJ0KNGxXr9Ss6A5wDaBXhZ6suLAy2zYaGrqmMTDkngKMzHHq/+zdd3xb1dnA8d+5kuW9V+zY2Xs5e5IdyATCHgkk7A0ltEBoX1ZLUwplbygEygwjEHY2IXvv6Sw78Y73lnTP+4djE8frypYsj/PlozaWju59JMvSozOeIyULv9zA+4vWl22ZerZH+3RKNtv2JPDah6t57q+X06NzGzdH6l7SloDMugVkMVXrV55NymzHkVk3Q+hilwzFy+KlUPBu5XNWogM2ZPZdEP5b2aIlI8e1nUBmzgJZdM5xqPRvmf8yAg0M9lQKUySEfg2FH5dtr6mX1yzUwHMiwvdmhEVtwqAozuZw8llQUEBERARQ1g2bnp5Ot27d6Nu3L9u3b3d6gK1RD//uRHpGkFaSXjZk6mCuNP760Ty38kl0Xa+ysv18pSVW8rMLDB+7tNhKXlYBwRFVPzCy0nL4x9UvsHvNfkxmE3abHaEJ1n69iXcf/Zhb/zmLyx6YVm3yJ6WkKL9s2z9vPy+nJYgd+7Rr0tMEXO3KmMuw6laWpi6rsvJdILBoFh7oem+TH3L/6OtN/PeL9QCVerb0s//OySvi/icW8e6/ZtE+pvXWYpSF74MsofbC6XawHYaS5eDl/B2+ZMH7lPUk1lG8XRZB0bfgO8fYcfNeOJt41v5NXOa/At5XIkzG9pUXmn9Zsup7O9hPA6WghRtOipVWpJWtdnclh4fdu3fvzqFDhwCIi4vj7bff5vTp07z11lv1LjaqVCaEYG7HGxFn//PoZnLoN9U5rj1QtaRSdcweJhzN8yxeVXtL8rMLmDf2cfatPwiA3Vb2AVG+8MdabOXNeQtZ9FzlqRk5Gbl8tmAx17W7k0sDb+TSwBu5vt2dfLZgMbln8hwLTKlCExqz2l/L3/s8ydjw0YRYgvEz+9HWuy3Xxl7FC/3/Te/AXu4Os1aZ2QW8/8W6WtvouqSkxMrbn65tpKiaHqkXQuE3GBsm0ZAF/3N+DPaksgVGBqssyKKvDB43HUqWYuyxSSj60tBxzyWECWFuhzB3UYmnUi1n727kigVMzYXDPZ8PPPAAycnJADzxxBNMmTKFTz75BIvFwsKFC50dX6vVK6AnD3a7nzeOvoX1Ejv5H9RdPF1ogk5929FtcGfD59E0jd6jerB/w2FDQ+8hUcHVtvv8X4tJik+p8xj/fewTxl87koh24Rzfm8DDk54mJyO30sKgjNOZfPB/n7H4lR/59/In6NA71vDjUarXzieWuR1vdHcY9fL98j0YmcZn1yVrN8eTfiaP8NDmX0JLShuUrEIWfgLWA4AEc3eE7/XgOanqkLn9NFBs8Og62A45OWLA7shWexKMzj11IKEFHVm6EcHdDsSiKEpjcrjnc/bs2cydOxeAQYMGcfLkSbZs2UJiYiLXXHONs+Nr1foF9eXl/i9w+4ibibkmss7hdyklNz1zvcND1jPvm2Yo8QTITM7imujb+OQfX1cMf5YWl/LjO8sNHUMIwU/vriAnI5eHJz1N7pm8aktFSV2Sk5HHw5OeUj2grdyOfQkVw+t10aVkz6EkF0fketJ+BnnmSmT2PWd3EMoCmQ3WLcjsB5BnZqLbkpDFK9Ezb0dPn4TMusPdYZetPHdFe+nYzmVlc14VxQWkky+tVIMK+0kp8fb2ZuBANSHbVTxNnowNH82oD0fwb15j1efr0ExapURPM2ugS+a9dxfDpjn+u7jgsqEMmNCHXb/tN5RAWktsLHz8cwrzirjt2dkc2X7c8LxR3a6z4futePp4VunxrK5tdloO9414DIuXBz7+3oy4eDBTbplAUHjDh8VyMnJJS8jA7GGibdcoLF7NZFvAVqa01LHybVZr8y73JmUxMmsu2OLPXnPu4zn792k7BhkXIrFibH/y82lgrloNosHMncu22dSN7DJmAk9jG5hIzZGFZCYwtXWgvaIojc3hnk+A//73v/Tp0wcvLy+8vLzo06cP7733nrNjU85h9jAz/5MH+OfPf2Xo1AGYPMpqL/kG+nDJXZN5d++LTJ47vl7HNplNPPXtwwydOgAoG743YtFz3xG/43jFQiGjCnILWfLmL4aK40sJSfEpnNibyP4Nh3n/b59xbcwdfP/WUofOea79Gw/z1BXPcVWbW7l78CPcHvdnrmpzG2//+SPOJGfV+7iKa0RFBGIy+JoEiAxr5kPuRd+fHRKvLaG0A9Zz/u0oHeEzux73q50QZoTP9Rj7aLGfbVs7KUuh4GMHorAjvK90oL2iGOTsXs9W3PvpcM/n448/zgsvvMB9993HiBEjANiwYQMPPvggCQkJPP30004PUikjhGDI5P4MmdwfKSV2m73WkkeO8Pbz5u9LHuXQlngevvBpCnOL6ryPyayx5M1fufSeKYbPI4QgODKIg5uO1CtOqUvsup1X7n4Xs4eJqbdMdOj+Sz9czfO3vIGmiUrJb2FuId+8/CPLP17Df1Y/RbsequekqZg2oQ+/rtlvqG2b8AD69YxxcUSuJQs/pmI3BJcwgam96/Zk97kJin4C+wlqTYx95iA8etR5OJn7NJT+YvDkJjB1BMsIg+0VRXEHh3s+33zzTd59910WLFjAJZdcwiWXXMKCBQt45513eOONN1wRo1INIYShxDP1ZDoLH/+cp6/+D/+49gU+/9dislKza2wfGB5gKPGEsrqf67/dQqd+7YnpHm1o1bxEMuG6Cwwdvy5v/OkDigqM97ru33CI5295oyyBtVWdXqDbdXLP5PHIRX+npKjEKTEqDTewTyyd24cb6v28fuYQNAd6SZsaKW1gO7u4yOlE2cUUiwj5ACFcM81EaH6IkI/Bo//Za87dIUMru/jehvCfX+expC3x7Mp1g8+HFoIIfhsh6jWopyi1Uqvdncfhv1Cr1crgwYOrXD9o0CBsNrXLSFNRWlzKv+e+xuxOd/PZgsWs/XoTa77ayPt/+4zrYu/g3Yf/V+1WqI4OoRcXFCOE4MoHZ9S5IlnTBL4BPky+aRzhsQ2vxVhcUMIqB0rrLHpuSZ2JiW7XyTh1ht8WOb77iuIaQgienX8ZIcG+1Sag5V96LrmwH5dN7t+4wTmdC+ermmIR/n9FhH7j2DaU9SBMoYiQTxEhX4L35WAZCpZR4Hs3Inw1mv9fDCWIsmgRDn1MBT6HMKvqGIrS1DmcfN5www28+eabVa5/5513mDVrllOCUhrGbrPz5BXPs/zjNSDLEiopZVnNzbO9fov+s4SX73q3ylZ0QeEBNRy1egFn59dNvXUik2bXvJ2jZtIweZTNLfXx9+GSu6cYnlta2zF3rt5rqG12eg7rl2yptsfzfEIT/PD2sgbFpjhXm/AA3n/uBi6b0h8vz8olhtpFh/Do3ZP5yx0XNvvdq4TwLFuw4zQ+iIgtiIjtiLBlCN8bEZqfE49fMyEEwhKHFvgMWsjHaCEfoPnfjzA5sHjIug9HEnKhu3bbUKWVU3M+naZeEwb/+9//snTpUoYPHw7Apk2bSEhI4MYbb2TevHkV7V544QXnRKk4ZOWna9ny847aG0n4+b0VTLx+NHHjeldcHRwZRN8xPdm37lCdK981k8ZFN44r+7em8ZeF99ChTzu+/M8SctJzEUJUJLd9R/fktn/fQPezNUin3TqRb17+kZz0XMNlnqo8BF2ntNhad0Mg9WSGoQVOZceVJMUn1ysmxXWCA3350y0Tuf360RyMT6Gk1EZYiB9dOoQ3+6SzEu9roeBNjNe1rIkAU1u3FEzPLyjhl9/28d3SXSSn5WAymejbPZrLp/Zn+IBODkyNcPQ5aOhzpig1c8UweWsddnc4+dy7d29FaaWjR48CEBYWRlhYGHv3/tEL1aI+DJqZxa/+hDhvQU11TGaNJW/8Uin5BLjiTzPYs+ZAnecRmmDa7ZMqftY0jWsevpQrHpzO1l93lZUxspjpc0GPKgt4AkL9eW754zw86Wmy0nIMJ4bn0kwmwg1upWgyO9bJr5lNdTdS3MLH28LAvi13y1Thcy2ycOHZrSQblkwJn8avvXzoWCrznv6K3Pyic6bi2Niy6wQbdxxncN92/PORmfh4G5hzau5WVufUaO+nyfgGG2VF/FcjSzeCLCnb593rUjVsryiNwOHkc9WqVa6Iw7DXX3+d5557jpSUFOLi4nj11VcZOnSoW2NqSnIz8ziy7ZihtnabzsYftle5fuSlQ7jsgWksfvmnau9X/sXi4YX3EhEbVuV2s4eZ4TMG1Xn+9r1ieXfvC/zy35V89/ovpCVkVBz//OkA1cdvN1xeKqZbNN7+XhTl1T2n1WTW6D2yu6HjKoqzCVMkBL+DzLr1bHH18xOvuvZNP9tG+IH3TJfEWJPktBweeGIRhcWlVeaA289+wdy+L5G/PvcdL/zflXV2Ugifq8sS8TqJsrqlHv0MxSmLVyBzHwc9nfKPQYmE/FeRnhMQgf9SW2wqVblimLyV9nw2qyWBX3zxBfPmzeOJJ55g+/btxMXFMXnyZNLS1DyfcsUOLhgqLS5F1yt/kAkhuOuFudzz8s0ER5a9AWsmrWKOZrteMfzjh/lOWbUeEOLP1X+5lE9OvMkPBR/zQ8HHzPrbFXV+KGlmjT6je9JlQMeK6wpsBaxMW82ixK9ZfOo79uXsR5dlj83Lx5OLbhyHZqr7JW+36Q6Vj1IUZxOWIYjQb8H7SuDcHkIP8LoMfO+mbPV6da9nEwgvRPB/EVrZHG6p5yFt8UjbcaQ0NlWlPj75djNFxaXotW0eoUu27DrJjr2JdR5PmLuA50XU/VElEX73GRpxk8W/IrPvBj3j7DW2sxc7IMt6QzOvR+r5dR5LUZT6MdTzefnll7Nw4UICAgK4/PLLa237zTffOCWw6rzwwgvcdttt3HTTTQC89dZb/Pjjj7z//vs8+uijLjtvc+If4mdoyL2cX7Avmlb1jV0Iwcz7pnLxXRex5ZednD6SjMlsotvgTvQc3s0l0yo8vT0BmP1/V3Js90k2LNlabQ+oZtJo0z6c//viQQCsupXPExaxOn0NdmlDEyaklOjohHuGc4nHDPa9G8+vC1cZmsc6eHJclakIitLYhLkjIvDvSP9HwJ4IyLIV61rZIj9pGYIseBtKN55zLzN4TUf43Y0wd0Ra9yIL3ofin6noQRVBSJ/rEL5zEFqI0+ItLCrl51X7Kno4a2PSBN/8stPQ9AkR+G9k9h1nh9/P7/Ut291J+D+G8Jpc57Gkno/Mfrj8pxpa2cF2FJn/GiJAfa4o51A9n05jKPkMDAysSDYCA90zFFFaWsq2bduYP/+P2nCapjFp0iQ2bKi+LE5JSQklJX/Ua8zNzXV5nO7m7efN8BmD2PTTdvQ6VnZrJo0Lb6h9ezuT2WRoCN2ZTGYTj3/1EItf/olvXv6R9MQ/turz9vNiys0TmP34lQSE+GPTbbx0+BX25R4oGzYD7PKPYcrkA6k8c/crkAe6vea/cpNZw27TGXRRHH/7Yl6TmbNstdpZs/kIy34/QGZ2Ib4+Fi4Y3JnJY3vj5+vp7vCURiA0P9B6Vr3ecxTCc1RZLUx7AmAGj24ILRgAWfQ9MucvlPWQnjN0L7Oh4G1k0TcQ8jHC3N4pcSYkZVJSaqzcnl2X7DucZKit0Hwg+H0oWoIs/OhsHVQAE3hNQfjMQVj6Gwuy6FugmLo/8XUo+gLp/yeEo/vVK4pSJ0PJ5wcffFDtvxtTRkYGdrudyMjIStdHRkZy8ODBau+zYMECnnrqqcYIr0m5/IHpbFiytc52UkouvstFu5w0kMlk4sp5F3PZA9M4uCme7LQc8rLyST2ZQXF+MUte/5VRM4dwMOxgpcTzXHqxJOP+AvRcWecUud6jejD7/66k//g+TSbxPBCfzCMLviUzuwBNExVDmVt2neSN/63hkbsu4qIxvdwcpeJuwhwL5y2SkaW7ziaeNb3wddAzkFk3QdgvTik4b6/ly1217XXji6mE8ACfKxA+VyD1nLLFWFqQw4mhLHGghJosgNKt4OmcTTGU5k+tdncehxccHT9+HJvNRteuXStdf+TIETw8POjQoYOzYmuw+fPnVyr9lJubS2xsy1/J2H98H2584mo+empRtbcLUbZn+oNv30Fs96a9jaTJZCKsbQgLH/+cnSv3opm0ikTswye+wCfOE7/HzJjbV12dXvSrFT3DwBCgWSMoPIABE/q64iHUS/yJdO57/AtKrWU9VufPoSsptfH0yz+V9f5fUPcWhUrrIgvepazHszZ2sJ+C4l/B++IGnzM6MgBNCHQDiwU1TdAuun5D/mULgeo5Aqfn4NA4p97yR8sUxR0cXnA0d+5c1q9fX+X6TZs2MXfuXGfEVK2wsDBMJhOpqamVrk9NTaVNm+qLFnt6ehIQEFDp0lrc8MRVPPTfu4loV7YaXWiiokcvtkdbnlr8sMP7ortDWkI69w2fz56ze3vrdh2b1V4xd7NwTwnptxRgO1m1FEvht6V1f/5StsBo7eLN5J7Jc2rsDfHS+ysotdprXbghgOffXmZ4qFNpHaSeCSXLMVaeSEMWfuaU8wYH+nLBkM6GtkHVdclMd+xGpYVg6E2hon2QqyJRmiNVZN5pHO753LFjB6NGjapy/fDhw7n33nudElR1LBYLgwYNYsWKFcycORMAXddZsWKFS8/bnE25aTwXzRnLjhV7SDyYhNAEnft3oPfI7m4bWo7fcZyf3l3OyQOn0Ewa3QZ1ZsYdFxLVKbLa9i/c9hY5Z/Jqnr+qgyyErCeKCF9YeecW2ynd8B+2btdJS8ggINTfkYfjEidPnWHnvlN1tpNAfmEJK9cfYmorXyBls+uYzvmC1arZT2G8PqgO9uNOO/Wsy4axduvRWtuYNEGb8EDGDutaaztXEF7TkKUGt+QVQWCpupW00oqpBUdO43DyKYQgL69qD1FOTk61e4U707x585gzZw6DBw9m6NChvPTSSxQUFFSsfleq0jSNQRfGMejCOLfGUZBTwD+ufZGtv+6qWNwDsPu3/Sx6/jum3TaJ+169BbPHHy/JU0eS2bZsd90H18F6UKf0gB1Lzz+G34VJVDsXtCYmj6ZRWH7b3gQExt6TTJpg256TrTL5TEjKZPEvO/l51T7yC0swmTSGxLXniqkDGD6gYytORB19W3fe6753tyj+dv80/vHKT2XLnM7rudc0QWiwHy88fiUe7vh7854OeQtA5lP7X5gGPrOcMhdWUZSqHE4+x4wZw4IFC/jss88wmcrePOx2OwsWLOCCC1w7Mfuaa64hPT2dxx9/nJSUFPr3788vv/xSZRGS0rSUFpfy6JR/cHhrWfH7c/dXLx8+//ndFRTlFTP/4/srkoZ1izejmTRj22+aoHiVtVLy6dHPRMk6m6HRR99AH9p2cWDPaRcqKbUZLpelSygpaX3D7r+s3sc/X/+lUoJjt+ts2XmCjduPM2Fkdx5/YBrm1rhTlakD4EXZqu46G4OHc7+YXjS6J+2ig/ni+22sXH8I+9m/30B/by6f0p8rpg0gKMDHqec0SghvCHoFmXUbZb3D1b23aODRH+F3ZyNHpzR1asGR8zicfD777LOMGTOG7t27M3r0aAB+//13cnNzWblypdMDPN+9996rhtmbmZ//u5JDm+Or7HhyLiklqz5by9RbJlQs/MnPyi9bXGSkQ12Anlf5BL5XWChZU3dippk0pt82CYtX0+jlCAv2q3Wu57k0TRAW4ld3wxZk047jPPPaz9W+nsoT0VUbDuHn68nDdzbNag6uJDQfpM8VUPg5dX/zsiN8Zjs9hh6d2/DEn6bz59snkZ6Zj9mk0SY8oEl8GRCeoyDkQ2Tuk2A7QlnPb3k5KjN4X44IeAwhVCkzRXEVhxcc9erVi927d3P11VeTlpZGXl4eN954IwcPHqRPnz6uiFFpxqSUfPvqT4aGkDWzxpLXf6n42TfI13AShgTNr/Iwq+cQE57DTLW+yjWTRmCYP5c/OMPYeRrBqMGd8fL0MNTWbteZ4qQhd5tuo9hebGhrU3d657O65+xJCUuW7SY5LacRImp6hO+tIHypfUhdA8tIsAx3WRy+Pp50iAklJiq4SSSe5cp2kPoBEfI5wu8+8L0NEfAkImItWuDfy3pIFeV8asGR0zjc8wkQHR3NP//5T2fHorRAZ5KzOHU42VBb3aazdemuip9HzRzCf+d/YuxEdvAZZ0Eg0IRWtq2mBsELfMh6rIiSjbbyzVAAKoa1Q6KCeXbp/xEaFezgI6sqszSTtOJ0NKHR1rstvub6DS36eFu4fEp/Pqthh6dyJk3Qq2sU3WtYrGWETbexMXMzy1JWcKLwBADeJm/Gho9mYsQEIrzC631sVzh8LJVDR1PrbkhZr/CSZbu5Y9ZoF0fV9AhTWwj5CJl1M+hZZ68tfy2d/UOwDEcEvdZq58YKIcAyECwDHVn/riiKE9Qr+czOzmbz5s2kpaVV2Rf8xhtvdEpgSstQUlhSd6NzWEv+2Hc6tntb+k/ow+7f9tc671MzaXTs244Fsx5jbfp6VqatItNa9oGr+QhCXvSmdKudgq9KKd1jBxvEdI3m6nsuZdw1Iyu29ayvg7mH+D7pR/bm7qu4zizMjAgdzsXR04j0cjw5vPW6URw6lsr2vYk1JqA+3hYeu6/+e9AX2Ap54fBLxOcfRZzz8VtkL2JpynKWp67k3i53MSC4f73P4WxHTqQZbqvrkiPHjbdvaYRHLwhbDsXflZVTsp+ibI7nQITvbLCMRgiHB78UpdVqCnM+16xZw3PPPce2bdtITk5m8eLFFRWAqrN69WrGjx9f5frk5OQay1Q2BoeTz++//55Zs2aRn59PQEBApW/NQgiVfCqVBEUEVtqdp8724ZWLR897907uGzafvOyCasstaSYNL19P5n98P8GWYEyaiSxrdqU2Qgg8h5jxHPLHy13TShjdfxie5oYlnmvSf+f94x9WSt4AbNLGuoz1bMncyiM9/kwnv44OHdfiYeb5v17Bky/+wG+bjlTbJq+ghMee/Y5Xnrqa4EBfh44vpeTVI69zLL+szM75VQF0dHSp82r86/xfz7/S0a+DQ8d3FUdnBDhS7cCdpJTEn0gnNSMXD7OJnl3aEODv+NBvodXKjpQkCkuthPr40L9NFJrPLITPLBdErShKYysoKCAuLo6bb76Zyy+/3PD9Dh06VKnWeUREhCvCM8zh5POhhx7i5ptv5p///Cc+Pu5Zsag0H74BPoy4ZAgbf9haaZV7dTSTxpSbJ1S6LqpjJK9uXMC/577G3rUH0cwamhBIKbHbdDr378AjH95L+16x6FJnacpyQwlHqV7KujMbuDCy/oX24/OP8v7xD5Fn/zufjk6JXsLzh17kubh/OTwMvz8+md83x9fa5uTpTB7+52LeXjALzUBx73KH8g5zIK/6bWnPJSUsSfqeB7rdZ/jYrtQhNtRwW5Mm6BBjvL07SClZ9vsBPl68mWMJGRXXm80aF47uyc1XjyQqou7dfLKLi3h180a+2LeHQusfowfR/v7c1H8Qc+MGYNJUL6eiNIgL63zm5lbeTcvT0xNPz6qdI1OnTmXq1KkOnyYiIoKgoKD6ROgSDr8bnT59mvvvv18lnophl/9pep2JJ6Is+Zx2+6QqN0V1iuTFNX/nnd3/YdZjVzDjjou49tHLeH3Lv3hjy7O071W2ZerxghNkWbOq3L8mG89scuhxnO/n5F+r9HieTyIptBewLqPqrmB1+eirjXVuxqLrkgPxKWzbc9KhY69KW41m4M9fR2dH9i6ySo0/r67Uu2sU7WNCMDJN0a5LLnVzfdu6vPnxGp5++SeOJ2ZUut5m01n6235u/sv/KiWl1UkvKGDmF5/y0a4dlRJPgKS8PJ75fTX3/Pw9Ngf2UlcUpRouXHAUGxtLYGBgxWXBggVODb1///5ERUVx4YUXsm7dOqceuz4cTj4nT57M1q1bXRGL0kL1G9OLW549O+xXTdKgmTQ0k8bfPn+QiNiwGo/TsU87bnzyau555WbmPn0t3QZ1rrhNSklasWPz+/Ks9d9Os8BWwPasHeiGdpIRrE77zaHjp6TnsmnnCUPTFUyaYPGvOx06/snCBIOxlyXQSUXGFo25mhCCW68dVefwu6YJxo/oRru29ds/vDEs+/0An367Bah+OoFdlxQUlvDQP77Caq25ZNI9P3/P6dwc7LU8KcuOxvPM8pXEn0gnr8BI/U9FURpTYmIiOTk5FZf58+c75bhRUVG89dZbfP3113z99dfExsYybtw4tm/f7pTj15fDw+7Tp0/nL3/5C/v376dv3754eFQuCXPJJZc4LTilZThecIINk9YQ/E9v8t4rwXasctITMzSKB5+9gz4X9HT42Dbdxu8Z61iWupzTRUkO3dennqvRAbJKsxxK3jJKzzh0/MQk4z2Ndl2y91AyVqvd8K4xzWMmZPXGj+jOfXPzeHXhakyaqLSLjjg7JWNA71j+ep/jQ1ONRUrJx4s3V8RbE12XpJ/JZ83mI0wc1aPK7XvTUtmadLru8wEf7dnJ0nd2YtY0JozsznWXDKZbAyolKEprI6hzMKpexwQICAioNCfTWbp370737t0rfh45ciRHjx7lxRdf5H//+5/Tz2eUw8nnbbfdBsDTTz9d5TYhhMu32FSal1OFp1lw4N9YdSveEzzwGm/GelDHdlJHmMCjm4atfR6ZHR1LzgCK7cX859BLHM4/Uufw9/kEgiHBgxw+ZzmzcOxPxyQcq3HoyPxNgMzsAm7680e8+PiVhBvYn769TyxpxWmGEmiBIMo7yqF4XO2aiwfTu3s0X/24nVXrD1UkoN06RXDl1AFcOLpnk6oreb7jiRkcPZluqK2mCX5Ysafa5PPrA/swCVFrr2c56SEoDpV4Z+isXHeQlesP8dSDMxg3opvD8SuK0nwNHTqUtWvrrpfsSg4nn+eXVlKU2nyW8AVW3VqR5AghsPQ0VdoGE+Djk58yLGQInqbKE6x1qbM7Zw/H8o9jFibiguJo79sOgLeOvkt8/lHA8VXNmtAYE17/+o9hnmH4m/3Js9U9dK+h0d3fsQ/4jrGhDlUJAEhMyuSBJxfx3+duwLuO3ZrGR4xjU+aWOo+podEvqC8hlobXQXW2Pt2i6dMtmvn3TCEvvxhPTzP+vl7uDsuQ1AzjUz50XZKSllvtbcn5eehGSwBIif3sn5ddlwgkT7z4A+9FzaZrB/eufFWUZsGFC44a086dO4mKcm+HQr3qfCqKEWnF6ZVqX9amRC9h/ZmNjI8YC5QNS359ajG/pi6jVC+taPf16W8JMgcyuc1F7MjeWe/Ybu4wB3+PunsIa2LWzEyMGM93Sd/Xmfjq6A6vqg8J8mXssK6s2XSk0rBybey6JCEpi+tf+ZSAbv50DQnl2j596RJSdcV3D//udPfvxpG8+Fp7P4WAS6Kbzu5P1fG0mPFsZluMeno49tZrsVTf3tNkrnPo/lzinF+1BJCSL5Zs42/3N90pCoqi/CE/P5/4+D+qoBw/fpydO3cSEhJCu3btmD9/PqdPn+ajjz4C4KWXXqJjx4707t2b4uJi3nvvPVauXMnSpUvd9RAAg8nnK6+8wu23346XlxevvPJKrW3vv/9+pwSmNH9GSvmUEwgO5B5gfMRYpJT85/BL7MnZW23bbFsOX5z60qFYNDR0dPzNfsxufz3DQ4c5dP/qXBg5kd8z1tU6/1Mg6BfYl54BVYdM6zLnyhGs23oUXdoN17eUQNLuDHZ5nWF94kne37mNKZ278vxFU/E5Z362EIIHut7Lc4de4HjBCQSiUhKtoSGE4O7Od9LZr5PDsSu16945EovFTGmprc62miYYEte+2tuGtY3h+8MG/86EwDO78gvJrkuWrz3AvNsm4uNde2+5orR2TaHI/NatWysVjZ83bx4Ac+bMYeHChSQnJ5OQkFBxe2lpKQ899BCnT5/Gx8eHfv36sXz58moLzzcmIQ18Ze7YsSNbt24lNDSUjh1rLpYthODYsWNODdCZcnNzCQwMJCcnxyUTe5XKlqWu4JOTnxkeEh8Q1J8/dbuP75N+5KtT3zg1ll4BPRkXPpZBwQMwa87r8M8oyeC5Qy+SUpxSkeDCH8nuwKD+3Nn59irTCYzasuskj/5rMSUGkpRzJY0Gzs4b1YRgaHQMH868Ag9T5ekOVt3KhjMbWZaygoSiRAA8NU/GhF/ApMiJtKnH7kyKMc+9vYwflu821LP9+Wu3EFPNFrAFpaUMfe9Nimx1vD6kxJIDYbuqv/njl29q8jVRldbNnZ/f5efufec/MXk6d2qPvaSYfW891uryEkOfwsePH6/234pSm2CPYIfmYurSjk238VPyz06P5YKwkQwLHeL044Z5hvHPvk+zI2sXq9JWk1ycgklodPHrzMTICXT27dSgvbOHxLXn/eduYNYDHzh0PyH/mEqkS8nG04ksPrifq3v3rdTOQ/NgTPhoxoSPpsRegk3a8TZ5oaltF11u7pXDWbPpCLl5RbUmoNddMrjaxBPA12Lh/8aM57GVy2o+kZSgQ+DRmpuoAvSKYkALmfPZFDjUBWS1WunRowc//PADPXs6XhZHaV36BfXFx+RNob3IUPtdOXt4fN9Thts7or1PO6cfs5xJmBgcMpDBIQNdcvx2bUMI8PMiN99YfUa7B8jzcgkNwUe7d1RJPs/lafKkYZuNKo4ID/Xn9X9cy5//8TVJqTmVFpiV//v6S4dw5+wxtR7n2j79sOo6T/+2Egl/LECSgABhg5C94JFf/f0D/LxoE956elwUpUFaabLobA4lnx4eHhQXqwLFijEWzYMLIyexJOkHwz2gzi5mLhB09utEjE+MU4/bmIQQXHpRHJ98u7nO1e8SKIyC87cA0pHsT08nu7iIIC/H9wxXXKNddAifvnoLG7Yd44cVe0hOzcFiMTGwTzsuvSiOtm2CDB3nhn79mdKlK1/u28uK40dJy8kn43Qu3mngnV55odG5NK3stWW0PqyiKIozODz57Z577uHZZ5/lvffew2xWi+WVMlJKsqzZlNpL8Pfwx9fsC5StlD5ZeJJd2XsMJaCOlkwy4sqYy51+zMZ22ZT+fPXTdkpKbDWW1pGANEFBdM3HKbLaCGoe1YhaDbNJY/TQLowe2qVBxwn38eXuIcO4e8gw7Had+5/4gj3pSTV+YTFpgsAAH66a7poee0VpaZrCgqOWwuHsccuWLaxYsYKlS5fSt29ffH19K93+zTfOXSiiNG2lupU16b+zLHU5KcWpwB8rvKdEXUSvgJ7c3/Vefk7+la9PLTa8K5BRJmHCLu2VFvvAH6u17+h0W71Wmjc1EaH+PP/XK3joH19jtdqqzBGUomyoPbMv6J7VzzE1CUGQl8o8WwOTSePZ+Zfz6L8Ws3P/qUo7QZWXZgoN9uOlJ64iJMi3jqMpiqI4l8PJZ1BQEFdccYUrYlGamSJ7Ec8ferGi0Hs5iWRPzl525ezmipjLuCR6BpMiJ/Dlqa+dHsP0qGn08O/GstQV7M3Zh1Va8Tf7Mzp8FBMixhHuGe70c7pLXK8YFv7nRr74YRs/rdxbsQJealDQBgpiwO5dc+I5pUs3vM/bDlcpo0udYnsxZs2MRWsZJYf8fD15+cmr2bzzBF/9vIMDR5Kx65LYqCBmTu7PpAt64OWpXg/uIGUxFK8EPQWEJ3gMRXh0dXdYSl3UgiOncTj5/OADx1bdKi3XW0ff5Vh+9dUPynshvz61mDBLGHFBNS90qS9/sz/ToibjbfKmd2AvoGz4vyGry5u6mKhgHrptEnffMIa0jDxO5+Uy59dvsNexWNkuJTf1b9zh1cScHA6fyQCgS0go7YOCGvX8RqSXpLM8dSW/pf9O0dmFbjHebZkUOZFRYSOafSJqMmmMGNSJEYPqrtWakZVPVk4hPl4WoiICHd7iVamblFZk/utQ+BHIfECjPKORHoMQAX9FePRxc5SK4nqGk09d13nuuedYsmQJpaWlTJw4kSeeeAJvb7V4oTVKLDzFzuwaigae59vTSxgWMgQvzYti3diCNS/hhY6dUmmt9vYgjyCe7PV/eJsqv/6ckXjmWvM4mHeQYnsJgR4B9AroiYfWtHqIvL0stI8JpT2hPK9NY96vPyGgyqQGQdlH22MXjGVgVC2TQZ1oW/JpXt60gbUJJytdPyImlvuHjmBYTGyjxFGXXdl7ePXI69ilvdKUjdNFSSw88REr01bxl+4PEdCAnbCagzWbjvDFD9vYtf9UxXVREYFcOW0AMyf3x7OG3ZUUx0hpQ2bfByWrOKcQ2h8NrDuQZ66FkA8QFueXhVMaTs35dB7D7yrPPPMMTz75JJMmTcLb25uXX36ZtLQ03n//fVfGpzRRv6WvqTLPsiapJakcLTjG2PDRLEtdUed9NDQmtZnAZW0v5deUZSxPXUmuLReBIMwzjEvbzmBYyFCn16LMLM1iUeJXbM7cjF3+EaOvyYdJkRO5JHqGUwvUO8ul3XsS6u3D8xvWsjs1pdJtnUNC+dOwkUzr6tje8vX1S/wR7v35e6r7CrDp9ClmLf6S5y+cysweVUu1nc7L5duDB0jOz8NiMjG8bQwTOnbG7IIalAmFibxy5FXsUq+yyK3851OFp3nh8Ms83uuxFln3VErJax+u5ovvt1Xp5UxJy+G1D1ezcv0hXvi/K/H1UUW4Gqzww/MSz/PpgA2ZdTdE/I4Qan620nIZ/iT96KOPeOONN7jjjjsAWL58OdOnT+e9995DUwWKW53k4hSHFg+lFKcwMXICK9JWocta9hJHYNbMTIgYh1kzMz16KtOjXb/vdHpJOn/fv4A8a16Vx1VgL2RJ0g/E5x9lXrcHKhJQu7SzM3sXq9PWkFKcikmY6O7fjQkR42jv67q6otW5oF17LmjXngPpaRzISEeXks7BIfRvE9Vo0xBOZmfzwC8/IKWs9pVRvkr/z8t+pld4ON1CwwDIKynhryuX8eORQwgh0M7Gu3DndsJ9fHhq3CSmdHHufLgfkn5El7LW6go6OscLjrM3Zx/9XDBtxN2+/XUXX3y/DaDKinh59n8OxKfw91d+4l+PXtb4AbYgUtqRBQupe4KfDjIHin4Cn+ZfpaPFUXM+ncZw1piQkMC0adMqfp40aRJCCJKSklwSmNK0acZfOhXtI70iuK/r3ZiFqdr7a2iYhYkHut5LqGfjbfUnpeTlw69Xm3hWtEGyP/cAi09/B0B6SQaP7XmcV468zt6cfaSVpJFcnMya9N95fN9TvH30Xax69VMGXKlneASX9+zNlb36MCAqulHnv368Z+fZhK52Avho904ACq1WZi1exE/xhysKpNt0HZte9nvIKCzknp+WsOTQAafFmWfNY0vmNkNfnjQ0VqStctq5mwq7XeejrzfW2U7XJWu3HOXkqTONEJX7JORk8+X+vXy8eycrjh2l1G537gmsO0FPNdhYIIuXOPf8itLEGO75tNlseJ1XpsXDwwOrtfE/YBX36+jbgb05+wz3frb3bQ9A/6A4Hu/1N35M/pktmVsr7m8SGkNDhjIjamqjF4Q/kh9P4tl9zWsjkaxIW8X4iHH888CzZJdmA1R6Dsr/veHMJuxS567Ot7foBVDlpJQs2rcHew01SM9ll5Kv9+/jybETeGvrZvanp9dauxTg4eW/MqZ9B6cUyE8pTjX8utXRSSis+7XR3GzdfZL0zBq2PDqPSRP8sGIP98wZ59qg3ODQmQz++ftq1iacrPSlKcjLi5v7D+KuwUOds/WonulAYwl6esPPqTidmvPpPIaTTyklc+fOxdPzj7k/xcXF3HnnnZVqfao6n63D2PAxLEn6oc525TsMxZ6TULb3bcfdXe4gz3o9KcWpCKCNVxv8PPxcGHHN1mVsMDx/tchexCcnPyO7NLvW9hLJpszNXBQ5kS7+DSse3hyU2G3klZY61H7FsfiK3tK6WO12vj6wn1sGDGpImMpZp1KyKxaj1cWuS06lZLs4osa3KzWFWV8vosRuq/I8ZBcX8+LGdexLT+O1qTManoAKHwfbu+e9UKmDGnZ3GsPJ55w5c6pcN3v2bKcGozQfoZ4hTIqcyLLU5TW2EWf/q2mHIX8Pf/ybwEribGvtieT5duXsNjxkuyx1ZYtPPg+fyeDjs8Pojrjrp+8Nt5XAz0cOOyX5bOMVafjLhoZGe592ZBYVsubkCXJLSgjw9GJ8h44ENuOC/SaTZvgzTwiBydSy5vWX2Gzc9v1iiu217xi29OgRPti5nVsHDm7YCS0DyxJQWWigsUB4TmzY+RSliTOcfKr6nsr5rm93DcX2Yn7PWFvjDkN3db69ye8w5KlZEAjDW3vapbH5YDo6B/IONiS0Js2m6zy5egWf7t2NqRGmFmSXGCvTVRd/D3+GhAyuNO2jJsVWwZ7DYQxf+jY2Xa/oLbSYTMzs3pP5F4xtlklory5tHGrf08H2Td2vR4+QUVh3IiiBD3Zu56b+AxvU+ymEN9L7aij8H1DX+4cJfK6s97kUF1I9n07Tsr7OKo1KExq3dJzLYz0fYUjIYLw0L0zCRLBHEDOip/Fcv38xJKSBPQZnSSnJLs0mvSSDEnuJU45Zrk9gb5fsKQ/GE9Xm6InVK/hs724AQ3M9G0IAoU6sKTwjehqa0BDVFoUqYyv1YN++rqw5nlyxAKr8UZba7Xx9YB9XLPqU7OIip8XVWLp1iqR750iMfGcwaYLpE1pW4fPFBw9UVFWoS3J+HjtSkht8TuF3D5hiAVPt7QIeR2ghDT6fojRlTa9oodKsCCHo7t+N7v6uqSNZYi9hdfoalqeuIK2kbBK+SZgYFjKEi9pcSEffDg0+x7CQoXxy8nPDBfCNEghCLa5btZ9eks6a9LWkFqeiCROdfDswKmwkvmbX79V9ICO9IvFsLDO6Oa8HvZ1PLH/qeh8vH3mtSpH58l7wEyc6UlRiqjGxtkvJyZxs/rpyGa9Pu8RpsTWWe24Yy5+e/hKQ1Pbd4YbLhxEU4OCcxSYutSDf0FzjckZ6SesitEAI+QyZ/SewbqIsCdUp6wOyg/BF+P8N4dM421dLWVq2y5LwQ4jmvZNXY1ELjpxHJZ9Kk5VrzePZg89xuuh0pX5Ju7Sz8cxmNpzZxM0d5zIm/IIGncfT5MncDjfw1rF3GxbweSSS8RFjnHpMKEvI3z++kI2Zm9HQkEgEgg1nNvJF4ldc2vZiLo6a7tJV9p/s2YVJCJf3eEJZr6e3hwczu1ctTN8QfYP68K9+/2BF2ipWp62h0F6WYMR6x9DXZySPZu6hrjExu5T8ejSe5Lw8ovzdP3/ZEQP7tuPvf76EJ1/4Abtdr5SMmTSBXZdce8lgbr5mpBujdA0/D8eSLR8P5+xwJkyhiND/Ia0HkEWLwZ4CwoKwDAPvGQjh2h0DpZRQugZZ8BGUrqXs9a0hPScgfG5AeI5w6fkVpZxKPpUmSUrJS4dfIakoudqP//KeqvePLyTcM6zB80pHhA0vm9914kNK9T9WbRtdmHI+DYGP2ZcRocMbFNf5bLqN/xx+icN5R4A/nofyaQM2aePrU4spthdzdazr5o2tTzjp1MSzppXXAoEQ8PKU6fh7On+XnTDPMK6JvYqrY66kRC/BJEx4aB68smmDQ8n1j0cONXxRihuMHdaVL9+6jR+W7+GnVXvJyinE28uDC4Z04bLJ/enaMcLdIbrE+I4d2Z6SZKj309tsdvrWtMKjJ8LDuV+m6iKljsx9HIoWUdbres4WnyWrkCXLkb63Ivz+0irKw9WLmvPpNCr5VJqkA3kHOVpwrM52AsF3p793yqKmkWHDGRAcx/qMjezO2UORvQh/sx9bs7Y7fCwdSZBHIJsytzA8ZCieJuckTqvT13Ao73CtbaSEj/b9zpebitifloUudWICArm+bxxX9OxFgGfDF8g4uwj3xI6d+e3kcWy6jknTkFJil5IOQUE8NW4iF7Rr79TznU8IgZfpj+clo7Cg7APYQHKiCUFGYYErw3OpsGA/5l41grlXtZ5er6t69eXFjevrTD5NQnBFz974WZr/sLTMf/Vs4glVFz2d/bngPdDCwfemxgxNaYVU8qk0SavSfjPU61i+ojytOJ0Ir/AGn9fb5M3EyPFMjBwPlA3x37rlznr1fp4uSuL94wv5MvErHur2IB39OjQoNillraWtAGw2jX2HYsnN80WItIrc6VhWJv9Ys4rXN29k4cwr6BMR2aBYYgMDSXFw3lx1BNAuMIi3Z1xKTkkxPx45THJe2d7uw9rGMLRtjFt6YXwcSDaklPg4OIzbmkl7EpSsLSs7pIWC53iE1rh1LcN8fPjb6HE8+dvKGtuYhCDSz4/7hzX/aQdSzytLLI20zX8DfGapeaDVEFIinDzVyNnHay7UanfF7TJLMzmYe4jDeUcosJX1IJ0qPOXw3vGuYBImBgUPdHg7UfhjKLzAVsC/Dj5HclHDYswozSCluOYt+qSE/Ydjyc3zqfj5j1jKLtklxcxe/CWncnMaFMs1vfs2OPEsj+svI0cjhCDIy5tZfeP488gLuH/YCIbFxLpt+G9suw4VK9zrYpeSsR06ujii5k/aEtCz7kCmj0fm/g2Z9y9kzkPItJHouf9AysatGnBj3AD+MX4SXmYzAioqH5hE2d9674hIvrzyOsJ8WsBiq+LvAYObQMgcKF7q0nCaLemiSyukej4VtzmYe4gfkn9iT87eiuvMwsSI0OEO9zS6Mkm5sM1EtmRtrff9dSSleimLT3/L3V3urPdxiusoMZWd40tObu0r3XUpKSgt5b3tW3lyXP0LWU/t0o1n160ho7DQ0LzI8+dPmoRAl5Inxk5gWlfnVkpILyxg06lEimw2Inx8GRHbDoup9vI25xseE0uHoCAScnJqTbI1IegRFkZcZMuqg+ls0nYMeeaastXV5841BKAYCj9GWndByEcuX3Rzruv7xnFxtx58e+gAGxITKLHbiPIP4MqevYmLbNNi5j5K2xHK5nnaDLQ2I23xtRQhU5SGU8mn4ha/p6/lv8cXVqmzaJN21mVsAIwv9hEIYrzbuiROgO7+3bg4ejrfJ/1Y72Po6GzJ3EaONYdAj8B6HcPfXPtq6qSUYMo+2Gv/2LBLyZf79/LwqDH1XsXraTbzwaVXcP03i8grKamSgGpnk8t5w0cyICqa/+3aycoTx7DpOj4eHszs0YvZfePoEdbwqRLlTufm8q91a/gl/nCleIK9vLmp/0DuHDwUs8FC4UII/j1pCrO+KZsjV10CqgmBh2binxMucs4DcBOpZ4I9A4QXmGIQwrkDYlJKZNY9ZxPPmuYK62Ddg8x7ARHwV6eevy7+np7c0K8/N/Tr36jnbdpU6lkdVWrJeVTyqTS6o/nH+O/xhciz/53PkV5PDY0Bwf0JtgRXuv5U4Wni84+ioxPhGU6vgJ5oDfhQvaLtZQR6BPLt6SXk2/IxibKeNEeKyOvoHM8/Qf/guHrFEGQJpJtfV47kx1f7vOXl+2D0Q6PIZuNEdha9wuu/mrlHWDhLrp3N61s2sfjg/kqLkPpFtuHOQUO4qHNXAEbFtkdKiVXXK/VC7klLZcvpU5Ta7bQNCODCTp3xMjueEB/LyuSqLz8jt5pEOKu4iBc3rmNnSjJvzbjUcAI6OLot/7vsKu77+QfSCwsqem/L/z/C15c3p11Cv2ba6ylL1iIL3ofSdVT0RmrR4HsD+FzvvB7I0s1gP2qgoQ6FXyD9Hmj0OaAtnTB3Rxrq9QSwITxcU7dZUcqp5FNpdL+mLDW8nWVt7QQCTWhcGn1xxXWH847wReKXxOdX/rAL9gji4ujpTIgYX6+hNCEEF0ZOZHz4WLZn7+B0YRIAazPWk1GaYfg4Nmn0A6B6k9tcyOH4I9Xe5ugXaGeUSooJCGTBxIuYf8EY9qenYz2bRHYKrrpDixCiIvHcknSKv/+2ir3paWiirP/bLiV+Fgs39R/IfUNHGE4SdSm5/ftvq008y0lg1YljvLl1E/cNNb6qe2jbGNbdfDsrjh9l6dF4coqLCfLyYmrXboxr37FBWy66k8x/HZn/MjbdzPq0tiQV+GMx2RkYmkIH/d9Q9D2EfFhWGL2h5yr+nrIhXyNf1IqhZDV4z2jweZVzeM2A3H8CBjbSEMGg9pavniq15DQq+VQaVaGtkC2Z2wz3blo0CyV6SZUheIHAQ/Pgga730t63HQC7snfz8pFXqx0izbJm89HJT0guTmVWu2vrPZfLrJkZGjIEzuZWp4uSyCzNNPx4wjzD6nXecoOCB3JhxESWpa2ocpu3Vyl5+SaM9H6ahCDGP6BBsZwrwNOL4TGxhtr+duI4t/3wbcXv6dzfV35pKa9t3sjhM2d4beqMGpO73JJifj5ymKT8PJLy8jiWnVXneSWwcOcO7hg01KE5oGZNY3Lnrkw+24vb3Mmi77HnvczCw31591B/0ot9OHe6xqiIRB6O20pv7QFEyMKGn1A/g7HEE0ADPbPh51QqEZof+N2FzH+x7rb+96uV7orLqeRTaVTZ1myHhtV1qfNg1/tZlrqCQ3mHsUkbQR5BjIsYw7jwMQRZggDIs+bxWvyb6LL6ofxyy1KX092/q9P2nB8bPtrQYiSBINo7ivY+7Rp0PiEEs9pfR4RXON8n/UiuLQ8NgQSi22RyKD6mzmOYhGBKl24EO3GvdKNyS0q45+fvset6jb8lCSw9eoRP9uzixrgBlW4rsdl4dv3vfLpnF1a7HZOmYTe4Kh3KhuDXJyYwrpWuTpdSoue9xmNbxvLViR780e3yxxeWDeltuXpFFB+O/ZEh/nsRHg3c1134YrznUwfRAlaXN0W+d5Yl9oUfUvX3cfZn33vB+3r3xNcMqDmfzqOST6VRmYRjLzmzZqZ/cFzFPEkpZbW9lmsy1mLVrXUO5WsIfklZ6rTks3dgL9p6R5NclFJrUi2RTtvyUgjBRW0uZELEePbk7CW1JA0TJtp2i+GutN9Jysutcfi5/Oy3DxrS4Djq49uD+ymyWg2NNL2/cxs39Otf8ZxZ7XZu+34x6xIT/9jRyYHEs1xaQb7D92kxrLv4Mt5yNvGE6nrJdalhBW5fO4W1UV/iF9aw5FN4jkEWLzHYWgPPhm2Xq1RPCIEI+CvScxKy8GMoWU5ZAuoBXlMRPrMRlv5ujlJpLZrnhCWl2QrzDCXAbGy4V0Oju3/lie81JW9r09cZmkOqI4nPP8qZkjOGYqgzRqExr9sDBFmCqq0FWr6a/+Ko6YwIc+5Wm2bNzIDg/kxpcxEXtplIr6Du/O+yKwn39UWr5nkyCYFJ03h5ygz6NrDIfH19e+iAoXYSSMjJYX96WsV17+/cxrrEBEO/59p4O2mf7uZI2o7x3qG4s33lNdOlRq7VwpKj2Q0/qdcUEEHUPR3EBJ4TEKbmuYCruRCew9CCX0VE7kdE7kJE7kULel4lnkaoOp9Oo5JPpVGZhImJkeOrlFiqjo7OpMgJho6bY811KA5H29cmzDOMp3o/zkVtJuFtqjyU3cm3I/d2uYsrYy932vlq0y4wiB+uu4H7hg4n5JxhdQ9NY2aPXnx3zSyn19V0REZhgUPvtZlFZYXH7brOwp07Gvw+rQnBsLZ1T01oiUpsNl7alsOxvGCkgb8/gWTxsdAGn1cICyLwXxVHrZ4JRADCf36Dz6cYI4RACO8WU8u0MZQPuzv70hqpYXel0U2KnMDajHWcKal5oY5A0DewD70Dehk6pqfJQoHd+P7aztprvVyAhz/XtbuGK2IuI7HwFKV6KSGWYCK9Gr+HMcTbhweGjeSeIcNJzsvDqtuJ9PXDtwnsT+1ncex5L495d2oKqQ0cLjcJweTOXYnwbX1lfPJKSpj73dfsSDG+mEeikVZU+6YFRgmvCRD0BjJnPshsyuYY6pT1f9jB1AkR/BrCbGzRmqIozZtKPpVG52f2Y36Ph3nu0AskF6dUWsle/u+4oH7c3fkOw7U5BwT1Z1Xab4YWMwV7BBHl5ZqhPYtmobNfJ5cc21FmTSM2sOGlcpzpok5dOHwmw9DWnMFe3hV70GcVGygRUwuTEPh7evLIqDENOk5z9cAvP7A71fHtXX08nff6EV4TwXMtFC9Flqw+u7d7GML7YvAYrHrglKZPlVpyGpV8Km4R6hnKM32fZmf2Llal/UZSUTKaEHTx68KkiPF09uvs0IfRhIhxrEhbVWc7gWBS5MQGFZxX6u/aPn15fcvGOttpQjC7X1xFSSS/evbalheEjwkI5L2LZ1Yk48l5eXxzcB+ncnMxaxqDoqKZ2qUbnuaW95a4Ny2V1SdPOHw/TUgmdOzu1FiEsID3DISq46korVrLe6dVmg2TMDEoeCCDggc2+FgxPjFMj5rKj8k/19hGQyPGJ4YLI1UBZXdp4+fP38aM46nfav6iYBKCrqFh3D7wjxX5/dtEEezlZbgHtHNwMGE+vkT6+TGhQycGRkUT7uNLodXKX1cuY8mhA2Vz3hAIAZ/s2cWTv63ksQvGcnXvvg1+nE3J5/v2VCThjpBScH2f+u3GpSgtVWudo+lsKvlUWoyrYq7AQ/Pg+6Qf0aVesSq6fCi/V0BP7u5yh9PneyqOmRM3EE+TmWd+/40CaykmISpKnNulZEz7Drxw0bRKc1QtJhOz+/Xn9S2b6hyy99A03r34Mn6OP8xHu3ay5NBBADxNJgI8PTlTVFQ2eibPjqGdPVxuSQmPrlhKsc1Wpb5oc3Y4I6Neu1nNGzGKtgHO24hAURSlnEo+lRZDCMFlbS9lYsQEfs9Yy9H8Y9h0G5FeEYwJH02sT+tc5dwUXdunH5d278n3hw+yJek0Vt1OtF8AV/bqXe3WnAB3DBrKquPH2J+RXmsC+ueRF3Dj4q9Iys+r1K7Ebie9sLDO2J5es4pJnToT7cQdoNypurJbtTEJwV9Gjua2gc6phasoLYaUZRdnH7MVUsmn0uIEePgzPWqqu8No9Y5mnmFr0mlKdZ3YgEAuaNe+0n7t3h4eXN27r+Fhbh8PDz65/GoeXfErv8QfKdsT/mxiZdN1Qry9+esF43hl8waSz0s8HfX53j3MGzGq3vdvSvpERLIt+bTh3s8vr7qO/m2iXByVoiitmUo+FUVxql0pySxYu4bNSacqXR/m48PtA4dwy4BB9V7Z7O/pyevTLuFUbg6LD+4nOS8Pi8nE0LYxXNipCz/FH+ZkTnaD4tel5Of4wy0m+by+bz/e37mtznaaEAyJbqsST0Wpgdpe03lU8qkoitOsT0zgpu++rraXLaOwkH+u/Y0jmWf418SLGlRaJyYgkPuGjqhy/Se7d6IJ0aBeTyib/9kSWO129qWnEeLlTWZxUY3tyn8TDw5vGQm3oriEKrXkNCr5VBTFKfJKSrjjh++wS1lr8vfl/r0MiW7Llb0atmd4dY5kZjY48QQq7Q7VXOWVlHDLksVsTT5d635GJiHQhOClKdMZ2kp3f1IUpXGp5FNRFKdYfHA/hdbSOr/IC+C9Hdu4omfvBhcWzy8t5btDB/hq/15S8vPJK214j6UmBJd279ng47iTlJK7flrCjpSksp9raXtZj17cN3REk9uQQFGaGqGXXZx9zNZIJZ+KojjFNwf3G2ongcNnMjialUmXkPrvHb4t+TS3LFlMbkkJAueMXgnAJDSuckGvbGPaknSa9YkJdbYzCUGB1aoST0VRGpXa5kVRFKdILyhwKAFMLyio97kOZqRzw+KvyC8tBZyXeAJc1as3S4/FszbhJHa9eXZLfLJnFyYDvcp2Kfn16JEG/S4UpdWQLrq0QqrnU1EUp/Dx8HBp+3M9v34tVru9wfM7zZqGlBK7lFhMJkrsdj7du7vi9ig/f+4aPJRZfeOa1d7je1JTDJdW0qUkPvMM4b6+Lo5KaeqkXgjFPyALPwXbMRAamHsjfGeD5ySEqP/frKKcSyWfitKMlNhLsEkb3ibvJrc//cSOnTiRnWUo6Qn28qJneES9zpOUl8uqE8fq1WGgIfDztPDWtEs4nZ9HYk4Op3Jz+O7QAUrt9irtk/PzeHz1Co5nZ/G30eOaTQLq6HPTSjtflHNI23Fk5lzQk6F8IosErNuQ2VvA3BtC/ovQqt8EQkob2BNAloIposZ2zZkqteQ8KvlUFAPSS9L5PX0daSXpmIRGZ7/OjAgdhrfJ9auiS/VS1mVsYFnqck4XlS0g8dK8GBM+mkmRE4j0ql8S52yz+vbn3e1b62ynCcH1feOwmEz1Os/OlGSHkqXydLFtQACz+/bnql59CD67mj2zqJALPngXXcpaj/nBzu0MiopmWtfu9Yq5sfUIC+NUbo6hLwIC6BQc7PqglCZL6pnIzBtAP1N+zTm3np16YjuIzLwZQhchhOWc++ZA4UdlvaUV9xdIz/EI31sQliGN8RBajTVr1vDcc8+xbds2kpOTWbx4MTNnzqz1PqtXr2bevHns27eP2NhY/va3vzF37txGibcmKvlUlFqU2Ev47/EP2JS5Be2cKdJrM9bzWcIXXBFzGZMjL3RZj1ieNY9/H/oPCYWJiHMK5hTrxSxPXcHKtFXc1+Vu+gfHueT8jogNDOTB4aN4YeO6GtuYhKBjUDC3D6z/B5LVwXmYA9pE8eVV11X7O/pq/z5KbLY6k1lNCP67Y1uzST6v7xvHr0fj62xnEoKx7TvSxs+/EaJSmqzCT0DPoCLRrJYdbPuheCl4zwBA2lOQmbPBfuq8+0oo+Q1Zsgr8/4bwvcGFwTeiJrC9ZkFBAXFxcdx8881cfvnldbY/fvw406dP58477+STTz5hxYoV3HrrrURFRTF58uT6Rt1gKvlUlBpYdSvPHXqB+PyjAOjnvTGX6qV8lvAFJfYSLm17sdPPr0udFw6/zKnC0wCc3zeno6NLnVfiX+eJXn+lvW97p8fgqHuGDMNiMvHCxnVYzw5jS8qSHLuUDImO4bVpM/D39Kz3OWIDjK/MNglB+6DgGr8cfH1gn6FeVF1KdqQkczo3l7YBTX/P91Gx7enfJqrWuZ/lz8g9Q4Y1XmBKkyOlvazXstbEs5yGLPwI4T0DKXVk1q1gP13Dfc/+/ef9HcztEZ5jnBi1ezSFYfepU6cydarx7aPfeustOnbsyH/+8x8Aevbsydq1a3nxxRfdmnw2rUljitKErEhbRXz+0SpJ3/m+Of0tyUUpTj//vpz9HCs4XiXpPZ+Uku+TfnL6+etDCMHtg4aw6ZY7eWz0OCZ37srEjp2Y3a8/3187m0+vuJoQb58GnWNAmyjaBwbVWji9nF1Krqll7/j0QsdWeWc42N5dNCF4d8ZMuoWGIaDKc2USApOm8fKUGQyIinZHiEpToaedM1xeZ2Ow7iv7Z+nvYDtMeZJZMw2Z/1YDAmwdcnNzK11KnLTL2oYNG5g0aVKl6yZPnsyGDRuccvz6UsmnolRDlzrLUpbXmXgCaGisTFvl9BhWpK2qNNRfEx2dbVnbybXmOj2G+gr08uKWAYN4Y/olvHvxZTwxdgK9IyKdcmwhBHcPGVbnb8YkBH0jIhkS3bbGNt5mx1bvejdghX5jC/Xx4aurruOpcRPpFPzH4g8vs5lr+/Tjp+tvZFrXbm6MUGkSZF3J4/nKvgzLwi8AI/O2dbBuRdpOOHieJsiFpZZiY2MJDAysuCxYsMApIaekpBAZWfm9NzIyktzcXIqKat5y19XUsLuiVCOlOJWMUmO9AeXJ36z21zk1hoTCxDp7Pc+NIaU4lQAP9w8JF9usrE04yZmiInw9PBgZ267BvZ3nu7Jnb+Izz/Du9q3V7uWuCUFb/wDemTGz1vm4Ezt24rO9uw0tzInw9aVzcPNawevt4cHsfv2Z1TeOvNISSu06gZ6eeNRzsZfSApnCAW/ASCIiwBRb9k/bMeru9TyHPQHMHRwOr7VITEwk4JwpPZ4NmJrUHKjkU1GqUWwvdqh9kYPtjWgeRX3+UGKz8fKmDXyyZyd5Z4u/Q1ktzYu79eCRUaOJ8PVzyrmEEMy/YCxxkW14d/tWdqX+Me0h0NOT6/rEcfugwQR51V6NYFa//ny8Z1ed59OE4IZ+AzBpzXOwSAhBgKeXu8NQmiAhPJE+l0HhFxhJJoXPrLP/cPQLTPP/wuPKOZ8BAQGVkk9nadOmDampqZWuS01NJSAgAG9v11drqYlKPhWlGv4ejq3+9Tc7f7VwO592ZJZmGer91NBo49XG6TEYVWyzMufbr9mWnFSlF9Km6yw5dID1iSf56urraevvvDfYaV27M61rd05mZ5NakI+X2Uz30DA8zcbe2rqHhnH7oCG8s21LjW1MQtAtNIyb+g90VtiK0qQInznIwq8oG1KvKbvSQAsC78vKfvQY5EDvpwnMPZwRquKgESNG8NNPldcELFu2jBEjRrgpojLN82u8orhYuGcYHXzaVypvVBOBYFRY5T9ku7STb82nVC+t4V51mxg53nDiOSRkEAEOJszO9Nz6tdUmnuXsUpJRWMi9P33vkvO3DwpiaNsY+kW2qTbxPFNYyFtbN3Pzd98w65tFPLr8V7YmnUZKySMjR3P/0BGYNQ1N/PEbL9+eckRMOz69/OoG7cikKE2ZMHdEBL8JeFB9D6UJRAAi+AOEVvblUfhci+HE02sqwhTqvIDdpbzUkrMvDsjPz2fnzp3s3LkTKCultHPnThISEgCYP38+N954Y0X7O++8k2PHjvHwww9z8OBB3njjDRYtWsSDDz7otKelPlTPZzMnpeRE4UnOlGTioZnp5NvR4V47pXoXtbmQd469V2c7TWiMDR8NwMmCkyxLXcGGM5uwSRsAXfw6MylyIkNDBmNyYKiqV0BPuvh15lh+zSveBQJNaMyInm74uM6WX1rKZ3t317nVpV1KdqWmsCs1hbjIxumllVLy5tbNvLRpPXZdViwg2yxOsWj/XvpGRPLOjJn8afhIbujXny/372VnSjJW3U67wCCu7tXH0E5Mdl1n9YnjLNq/h4ScHDzNJobHtGNWnzhiA42XhlIUdxGeoyHsO2TBQihaDJz94iz8wedahM8NCNMff7fCoxfS62Io/pGayzRpICwIv7tdHH3rsXXrVsaPH1/x87x58wCYM2cOCxcuJDk5uSIRBejYsSM//vgjDz74IC+//DIxMTG89957bi2zBCCkdHbF1KYrNzeXwMBAcnJyXDK3ojFJKVmbsY4fk38mufiP+W4mYWJYyFAuj7mUcM9wN0ZYVXpJBofzjmCTNkIswfQK6OlQMtbYpJT89/gH/J5RfdH08j6yuzrfwbDQIaxKW82HJz5GIColixoCHUmfgN480O1eLJql2uNVJ9+Wz/OHXuR4wQkEotLqew0NkzBxf9d76BdUczkhV/v24AHmLTVW6sksNGb1i+OJsRNcHFWZVzdv4MWN62u83SQE0f4BfHvNrIpdjxx1Mjubm5Z8zYns7Ip6puXH1qXk1oGDeWTUGLRmsjWnoki9EPQUwASmqEo7GlVqJ0uR2X+Bkp/L2lb0hJ59rQs/RPC7CEvDp6y48/O7/Nwjpj6N2cO5c6dt1mI2/Px4i8hLHKF6PpshKSWfJnzO0tTlVW6zSzsbz2xiV/ZuHuv5MDE+MW6IsLJThaf4IvErdufsqXR9oEcg09pM5qI2Fza5fcqhbJHGzR3n0sarDT8l/0yBvRANDXn2vyivKK5vdw19g/qwI2snC0/8D6iuGHzZz/ty9/PesQ+4u8sdhmPwM/vx156PsunMZpamruBk4UkAfE0+jIsYy4SIcYR5hjnpEddPemF+tSvOq6MjSS9wbq3MEpuNXakpFFqthPr40Ds8Ak0IEnNyeKmWxBPKemOT8nJ5c+smHhs9zuFzpxXkc/VXn5NZVFhxvHOPDfDu9q1IKQ0dv9Bq5Zf4w2WJrCboF9mGMe06NNuFTkrzJDQf0DrV3U5YIOglsM5GFnwMpRsAK2hRCJ9rwHsmQmtBPf/nlEZy6jFboWaTfD7zzDP8+OOP7Ny5E4vFQnZ2trtDcpv1ZzZUm3iW09Epshfx/OGXeL7fvzBr7vs1H80/xrMHn8eqW6vclmPN4bPERSQUJnJbp1tctkVlQ5QNaU9jcpsL2Zm9i7TiNEzCRGe/znTx64wQAikl35z6tkrP5Pkkkk2Zm7ms6FKivI0PO3toHlwQPooLwkdh023YpR2LZmkyz5eX2QOjAygC59XKzC0p4c2tm/hs725yzynIHBsQyC0DBpGUl4d2Tk9kTexS8vm+PcwbMQovB+t+vr5lE5lFhXWe470d27i2T79K9TYrxaDrvLp5I+/t2Eqh1Yr5bLJp03Xa+Pnx2AVjmdFNLdhQmh4hBFiGqD3cFYc0m6/TpaWlXHXVVdx1113uDsWtpJT8lPxLnQthdHSySrPYlrWjkSKrqlS38uLhl7Hq1loXzqw7s4HV6b81YmSO89A8GBIymOnR05gSNZmu/l0qkr8ThSdJKEo0XJB+Vdrqesdh1sx4mjybTOIJcEG79oa/vNul5IJ2Dd8GNLOokCsXfcq727dWSjwBTuXm8NRvK/l07y5D9TuhbN7qnrTUuhueo6C0lK/27zV0DpMQfFJDSScpJQ8v+4VXNm+g0Fr2Jc2m69jO7mGfkp/P/b/8yKcGSkIpiuI65aWWnH1pjZpN8vnUU0/x4IMP0rev8bltJSUlVbasau4Si05xqui0oURHIFiT/nsjRFW9zZlbyLPlG1qx/XPyr4Z7z5qa00VJhtvq6JwqOu3CaBpfx6BgRsW2q1gdXhMBBHp6MaVz1waf84FffuR4dla1Q/3lI2P5pY5VGii22hxqvy89jSKbsfvYpWRtwslqb1ty+CCLDx2o8xiPr17ByVY84qMoSsvRbJLP+liwYEGl7apiY2PdHVKDZZZmGm4rkZwxuEuPK6zLWG+oVBFAakkaJwqr/3BuaYw+J83J42Mm4Gk215iAll+7YOJFhmtw1uRgRjrrEhMM92oaFe7r61D7EoOJZ0V7e/XtP9i53dBiJAF8tlf1fiqK2+jSNZdWqEUnn/PnzycnJ6fikpiY6O6QGsxDODYnzcOBldXOllWaZaiHtlyONceF0dStxF7C6rTf+Mf+BTy08xH+uudxFiV+RXpJRq33i/Wuee/w82loxDaBRWDO1jU0lEVXXkukX9kORuVJaHlK5eNh4dWpFzOlS8N7Pb8+sK/OXlZHCKBrSCjdQx1buNXGz3hJs/LtPs+Xkp/H7tQUQ4u17FKy5NBBh2JUFEVpity64OjRRx/l2WefrbXNgQMH6NGjfhPtPT09m/X+qPm2fHKtuXhoFkItIWhCo5NfRzyEB1ZZdQHP+TQ0+gT2aoRIq+elOVaSwlNz3+/qcN4RXjz8CoX2wkoLh5KKkvkx+WcubzuTS6JnVDvXsr1ve8IsYWSU1p6kQtmw+7iIsU6PvynoFR7Bb3NuZfXJ43x/+CDpBQX4WzwZ37ETF3fr4bQi7Ul5eYaStXKC2heUSuDWgYMdnkfbNTSU3uERHMhIrzMeXUqu7l11ylB2sWPbsuaUOH8bV0VRDFKr3Z3GrcnnQw89xNy5c2tt06lT3eUeWpoDuQf5JWUpu7J3VyRBoZZQJkVOYGLEeC4IG8lv6b/XOZdSR2dCxLhGiLh6/YL6cqLwpKHeT0/Nk06+HRshqqpOFiTw74P/qSgKf2685c/xN6e/xayZmR41tcr9D+YeMpR4AowMHUEbr0iHY8yz5pFSXLYgpo1XZJPdSMCkaUzs2JmJHTu77BwWk6miyoARZk1Dl7LGYfpre/flyp696xXLHYOGcP8vP9baxiQEoT4+1c519Xfwy7Gfpfl+mVYURSnn1uQzPDyc8PCmVQjd3X5K/oUvEr+sqCdZ7kzpGRYlfsX6jA3c2ek2tmXtIL+OxTzTo6a5tdD8uIgxLEn6oc52Ghpjwi/A0+SeD9ZFiV9hl/Y6k+RvTi1mbNho/Dz8Kl3/7eklaGiGFlaNCbvAodgSChP5MeknNmdurTh++Xaa06Om0d63XaX2dmlHSunW8lquNrRtDN8ZWKBT7qXJ0/ly/15+O3m80m+4jZ8fdwwawo39BtS7esCMbj3Yl57G29u2VNvDahICX4uFDy69otq5rtF+/nQPDePwmYw6v6KZhGBa1271ilNRlIYTOH91estbAWBMs/mESkhIIDMzk4SEBOx2e8W+pl26dMHPz6/2OzcTWzO380XilwDVJjISyemiJD48+TGP9XyEFw6/TFpJWqXEpzxpnRE1jctjZjZm+FWEWEK4KuZyFp36usY2GhrBlmAuib64ESP7Q2pxGntz9xlqa5c6azLWMi1qSsV1acXpHMgzNg9PIFiTsZaegcamkezJ2ctLh19Fl3ql14OOzpbMbWzL2s79Xe+ju39X1mWsZ1nqiordrkItoUyMHM/Y8NH4mVvG30e5S7r14Jk1qym01T71RBOC3uERHM7MYF96akVy18bPj+ldu/OnYSPwdUJP4iOjxtA9NJw3t27iSOYfC/zMmsbF3Xpw/9ARtA8Kqva+Qgjm9h/I/BVL6zyPXUqu7xPX4HgVRamneuzFbuiYrVCzST4ff/xxPvzww4qfBwwYAMCqVasYN26cm6Jyru+SltRZqFxH53D+EQps+Tzb7xl2Ze9mTfpa0ksysGge9AroyfiIsYR6hjZi5DWbFjUVTZj46tTX2KVe8djKE+Z2PrE80O0+Atw0jHwkP95wW4nkcN6RSslnkgNlliSSxEJji94ySjJ4+fBrNfbI6ujoEl45/BoBloAqVRDOlJ7hy8Sv+Tn5Vx7u8RDtfJp/pYdyvhYLj40ey99W1bzRgiYEmhDEZ55hX3papTmZqfn5/HfHNtYmnOSjy64k3MexVe7VmdmjJ5d278H+9DSS8vLwMJnoFxlJiLdPnfe9omdvlh2NZ9WJY7X2fv55xAV0DW3437WUkozCQkrsNkK8fQzPxd2blspPRw6TU1KMn8XChZ26MCgquknVnFUUpXloNsnnwoULWbhwobvDcJmTBQkkGExMNDRWp6+hi38XBgT3Z0Bwf9cG1wBCCKZGTWZ02Ch+z1jHwbxDWHUroZ6hjA4bRVe/Lm798LJVs/NSbars1ORg6EYf64q0VYamAtiwkVWaVe1tEkmBrYB/H3yef/R5miBL89/mrsRmI62ggNHtOvDYBWN5dt0aJFQkl+VD315mM3Zdp8Rur7IYqPyn+MwzzP32a769ZhYeJlODYxNC0Dsikt4Rjs3pNWsab0y/hGd+X81ne3dj1yUmrex1YtN1/C0W/jxyNDf069+g+IptVhbt28vCXds5cbZeqEkIpnbpxs0DBtG/TVS19zuZnc2ffv2RXakpmISoeA2/u30r3ULDePGiqfQMj2hQbIrSHLiiKHxrLTLfbJLPlq58MYkROjpJRckujMb5/Dz8mBo1malRk6u93abb2JWzm6SiZDSh0d6nHb0Cerp8z3dH9kXX0Kq0j3GwzFJ7n7p395FSsjptjaE5pFB1L/lz6egU2ApYmbbK7dMwGuJUbg4f7NzOF/v2VOwC5G+xcE3vvnh7eLDxVCL5paWE+/hyWc9e7ExJ5psD+2pdhW6XkgMZ6by3YytnCovIKi7C18ODiR07M7p9B0O1N53FYjLx1LiJ3D90BIsP7udkTjZmTaNvRCTTu3ZvcG3U7OIibvz2K/alpVW63i4lP8cf5scjh3hq3ERmn5fgJuRkc9miT8g7u4uU/bxhx6OZZ7jyy8/48qrr6KUSUEVRDFLJZxPh6Aedq5OyxiKlZHnqSr5L+p48Wx7a2dKzOjqhllCuib2SYaFDXXb+XgE9CfIIItuaXWdbHZ0x4ZUXDIV5htE3sA/7cvY7rfpAqV5Kob2wznZG6UhWpK1iZttLmuXrZlvyaeZ++w3FNmulFet5paV8sW8Pvh4WPrrsSvpFtgHKtr18+rdVhovQP7d+LWZRNldaCMHHe3bR1j+AFyZPZUh049ZkDfXx4daBg516TCkld/24hAPp6dV+TSl/nh5fvYK2AQGM7/BHhZHHViwlr6SkxufSLiWldjt/+vUnfp01Rw3BKy2bKrXkNM3vk6iF6uDbwXBbDY3Ofi2jBNUXiV/yccKn5NnygLNzGc8mcWdKz/DG0bdZkbrSZefXhFZpDmeN7dDo7Nup2nJQM9teYuj+cYH96ORXdzkpk2j4EPD5ymrG5jn9uK6Wkp/HTd99Q9F5iWc5u5TkW0uZ8+3XZBSWJeyJuTk17iZUE5vUsUtZsZ96cn4es7/5ki1Jpxr+INxsR0oym06fqjMZ14Tg1c0bK34+lpXJ+lOJdd7PLiXxmWfYlmx8/rOiKK2bSj6bgJSiFA7mHiTaK9rQ1os6OuPdWL/TWXZn7+HnlF/rbPe/k586tH+6oy6KnMSYsNFA9Vtflg23h3J/13ur7dnp4teZe7vehVmYK3puy5Ufr7t/N+7ucoeheMyamXbesS7YhrP5fcX+ePcuCq3WWofPdSnJKy3hi327Aec8yvK6oPN+/dmhgvZN0Wd7dxvaEUqXkp0pyRw+U1azdvmxo4ZHZMyaxrJjxhfvKUpzJKR0yaU1UsPubnQ8/wRfJH5puFQPlCUzY8JHO1yoPKMkg325Byi1lxJkCSQuqB8WN269CbA0dbmh+pgCwcrUVdzQYZZL4hBCcHPHOXT178zPyUtJKv4j0fUxeTMufCwzoqfha655VfSg4IEs6PsPVqat4rf03yuGzbv4debCyIkMDhnkUI/mhW0m8t/jC+v9mM7nY/JpsoXpa6JLycd7dhpK/nQp+d/undwzZDixAYFYTCZK7fYGn/90Xi6/nzzB2A7u2QDBGY6cyTA8BQHgWFYW3ULDyCstQRPCcPKde3ZeqKIoSl1U8ukmB3IP8vyhF9GlsUUl5Una8NBh3NjeeBKWUpTCJwmfsztnD0BFKScvzYuJkRO4vO2lbilIXmQvYk/OXkNtdXQ2nNnosuQTyhLQMeGjGR12AaeLTpNtzcFT86S9b3ssmrFSNBFe4Vzb7mqubXc1pboVszDVe47l8NDhrEhdRUJhYo3JeV1lucppaIyPGOeS4XxXkVLy73W/O5TQpBUUUGKz4WexMLN7T74+sM+hpKs6ZqGx/PjRZp18OvoaLO8lDfT0Mpx4SgmBXo5tp6sozY5+9uLsY7ZCatjdDYrsRbx85FXs0l5nr59JmPAx+TAoeCDzezzMHZ1uNZwsJhae4sn9f2dvzh9F1MuTlWK9mJ+Sf+Y/h1/Cpjs2P84Z8m0FDrUvsBca3k6xIYQQxPjE0CewN139uxhOPM9n0TwatLjHonnwl+7z6Hh2LvC5w/nl/27nE0ugR2CVof5zCQReJk8mRY6vdyzu8MLGdbyzfYvD9ysfJr5t4GDMmqnBExd0JPmlpQ08inv1i4w0NOwOZaWqep7ddW5Kl66G/+bsUmd61+71DVFRmgU17O48Kvl0g3UZGyiyF9fZayUQtPWO5s1Br3Jv17voEdDd8GpSXeq8dPhVSuylNSa4EsmB3IOGtsB0Nm/NsV4ST82z1a2k9fPw42+95vPnbg8SF9SPEEsIIZYQ+gX1YV63P/Fk7/9jfo+HCfAIqHGuqrfJiz93n0eIJcQNj6B+TmZn88aWTQ7dRwCdg0Mq6nV2DgnlvUtm4mk2G068qqMhCPb2rvf9m4Lr+8YZ6gE2CcEF7doTE1BWDzYmIJDxHTrV+fyZhKBvRCR9HaxvqihK66WG3d1gbcY6Q+0kkoTCRJKLUojybuPQOXZn7yGjNMPQOZanruTi6Ol41LOXrz78PPzo5NuR4wUn6kzCNTQGBg9opMiaFk1o9A3qQ9+gPtXeHuXdhmf6PMXq9DUsT11B1tmSUb4mXyZEjmNCxHhCLMGNGHHDfbZ3F5oQDg+Zz4mr/BoZFduepbPn8r/dO/l8727yzvZgtvUPICkv19DCJJvUubiZ9+h1Cw3jku49+OHwoRqH0QVlvf4PDh9V6fpnJlzIZYs+Ib2goNrfh0kI/CyevDh5mitCV5SmRZVachqVfLpBdmm2Q+1zrDkOJ58bzmwytJgHoMBewP7cA8QF9XPoHA11YeQk3j72bp3tdHQmRTSvYePG5Ofhx4zoaUyPmkrh2ekJPmafZlnTE+C3kyccSjw1IWgXGMhlPXpVuS0mIJD5F4zl4ZGjySkpxqyZ8LdYmLf0J344fKjW85iEoFtoWI07/zQnz06cTLHVxtJj8ZjOS+w1IfDQNF6fdkmVxxrp58fiq2fx2MqlrD5xHHF221JdSnQpGRzdlgUTL6JDUPP6gqMoinup5NMNPE2e4MCujp6ap8PnyLZmG94hByDHmuvwORpqeOhQtmRuYUf2rlp7Py+KvJAu/l0aMbLmSQhR64r85qLY5tgc5AgfXz6+7Cp8LTVXbzBpWqV91p8YO4Hdqakk5GTX2qP36tQZLWK6h6fZzBvTL+H3kyf4aPdO1icmUGq3Ee7ryzW9+3Jt735E+VdfDSHSz4//XnI5iTk5/Hr0CFnFRfhbPJnUqTNdQhq+17yiNBvn7fDltGO2Qir5dIP+QXEsTVluKDn0M/sR6+P4LiteJsfmVHqZHE9wG0oTGvd0uYtPEj5jddqash1mzs5dlOiYhZmLo2dwSfSMRo9NcZ8oP38Sc3MMr7R+a8alRPsHOHSOIC9vvr76Op5cvZIfj5QNR5s0raJHb0RMO54eP7FF9ehpQjC2Q8eKlftSSocS69jAQKfvvqQoSuukkk83GB8xjl9SltbZTkMwIWJcvUoh9Qvsy87sXYbamoSJnv49HD6HM5g1M3M63MCl0ZewNmMdycUpaAja+bZjVOgIfMw+dR9EaVGu6NWbjacT62wngA5BwfwSf5j3dmzFopkYHN2WS7r3xMej7vnLQV7evDRlOn8dM46lR+PJLCrEz+LJ+A4dW1TSWZOW0KOrKI1JyLKLs4/ZGqnk0w3aeEVyafTFfJf0fY1tNDQivSKZ2mZyvc4xMmw4nycuolSvvUyMhsbQkCFuL0AeZAlkRrRatKDA9K7d+Nfa38gqLq6191MCx7OzeHf7ViRlyeg3B/fzjzWreeSCMdzQr7+h84X7+DKrb5wzQlcURVEMaJ4rElqAy9peypUxl2MSpkplcsprNnb178JjPR+pd8+ft8mbmzrMqbWNhoa/hx9Xx15Zr3Moiit4mT347yWX411DmaTzr7GfHSovn7tZaLPyxOoVvLd9ayNE6zrpBQXsT0/jWFZms9/iU1FahPI5n86+tEKq59NNhBBcHD2dceFjWJOxliN58dikjXDPMMaEjaajX4cGn2Nk2HAEsPDE/yjWiyt2xClfBd/Wuy0PdLun2ZXiUVq+fpFtWHzNLF7YsI6lx+IrJV8Bnp7klpTUWaFkwdrfmNqlG20DHJsP6m4rjh/l/R3b2HDqj6kHUX7+zIkbwOx+/Q1NKVAURWnKhGyMbWOaiNzcXAIDA8nJySGgmX0gNUSJvYRNmZvZm7OfUr2UII9ARoaNoKtfFzXvS2nyUvPz2Z6ShNVuJ9LPj3t/+p4zRUV13k8TgjsHDeXPIy9ohCid4/n1a3lj66Zq91Qv230ogk8uu0ptZam0Ou78/C4/97hhf8Nsdu7fns1WzOpN/2h1eYnq+WwFPE2ejAkfzZjw0e4ORVEcFunnx9Qu3QDYk5ZqKPEE0KXk16NHmk3y+dX+vbyxtWxnp+qG2SVwKCOd+37+gY8uU1NlFKXRqVJLTqPmfCqK0mwUOLjPenPZl11KyaubN9bZzi4laxNPsj89rRGiUhRFcQ2VfCqK0myEOLDPugBCfZpHqa4tSadJzM0x1NYkBJ/v3e3iiBRFqUK66NIKqeRTUZRmo2tIKJ2DQ6qseK9JdVtuNkUnsrMMt7VLyXEH2iuKojQ1as5nK1NsL2ZH1k6yrNl4aB70DuhJtHe0u8NSFEOEENwyYBCPrVxWezvKtpS8smfvxgmsgcyaY/0AHprJRZEoilITISXCyXM0nX285kIln62EVbfy9anFrExbTYlegoaGPPtfN7+uzG5/Pe1927k7TEWp09W9+7LhVCLfHz5Y7e0aZUnqq1NnNJtV4XGRbQy31YQgro3x9oqiKE2NGnZvBWy6jRcOv8wvKUsp0UsA0NGRZyebHMmP5x8HFhCff9SdYSqKIZoQvHDRVB4cPhJ/iydQNg9SO1s2rGd4BB9fdhUTO3Z2Z5gO6RwSytDomIrHUJdre/dzcUSKolShisw7jer5bAV+SP6JA7kHK5LN80kkVt3Ky4df48X+z9VrL3lFaUwmTeO+oSO4feAQVhw/yqncXDxMJgZGRTvUi9iU/GXUBVz39SKkrOkvtWw6wa0DBxPp59eYoTmVlJJtyUnEZ55BCEHv8Aj6RES6OyxFURqRyjJaOJtuY1nqihoTz3ISSa4tl21Z2xkWOrSRolOUhvE0m5nWtbu7w3CKQVFteXvGpdz94/dYdXulWp8moWGXOrP79efhkc23Xu/3hw/y0sb1VRZM9Q6P4C8jRzOmfQf3BKYoRkhAd8ExWyGVfLZwh/IOk2/LN9RWINhwZqNKPhWlkZTYbPx45BD/272Tw2fOANAlJIT2gUHsS08jo7AQbw8z4zt0ZFbf/vRrpr26AG9v28yz636vtlLB/vQ0bvrua56/cCqX9WweFQqU1kctOHIelXw2gJSS+Pyj7M3dd3bbyiCGhQwhyBLk7tAq5NryDLeVSHKsuS6MRlGUcqfzcrlx8Vccz86qtJ3mgYx09qWn0Tk4hKWz5xLl7+/mSBtuS9Ipnl33O1B9R0/5dQ8v/4X+UVF0DAputNgURWl8Kvmsp/i8eN4/8SGni5LQ0BBCoEudzxMWMTRkCHM6zMbH7P4C116ap0PtvU3Gi3grilI/+aWlzPrmS06fLSx/7hB7+b9PZGcxe/GXLLl2Nr4Wi1vidJaFO7djEgK7gV6eT3bv4m9jxrk+KEVxlMQF22s693DNhVrtXg8Hcw+x4OC/SSpKBspWjtulHYlER2dz5hb+eeBZiuzG9qB2pR4B3fEQHobbDwju77pgFEUByvZxT8zJrjUZs0vJiewsvjm4vxEjc74iq5WlR+MNJZ52Kfn6wN5GiEpRFHdSyaeDbLqN1+LfxC71Ghfx6OicLkri61OLGzm6qrxN3owOH4Vm8FedZzU+TK8oiuOklHy4a4fh9h850LYpyiouMpR4lsspKanUE6woTYYqteQ0Kvl00Las7eTZ8upcPa6jsyZ9LcX24kaKrHoFtoKKgvJGfJf0PRvObHJxVIrSehVarZzMyTb0FymBo1mZlNhsrg7LZXw8jI+8AFhMJsP1ThVFaZ7UnE8HbcncikAYSuZK9BL25e5nUPDARoisqhxrDs/sf5b0knTDySfAt6eXMDxkKEJ9ACjNkE3XWXH8KJ/v3c3x7CzMQmNwdFtm9+vfJOpJOtILWK459wQGeXnTKzycg+kZ6HW8D5mEYEy7Do0TmKI4SodqyzU09JitkEo+HZRny3cokSuwFbgwmtq9duRN0kvS0R18dacUp3A0/yhd/Lu4KDJFcY1TuTnM+fZrjmdnVVrgcjInm0X793Jxtx78e9JkPM3ue+vzt1gI9vIiq9jYqEiotw9eboz30JkMDmWkA9A9LJzuoWEOH2NO3EAeWf5rne3sUnJDXH+Hj68oSvOikk8H+Zp9Dfd8AviY3LPi/Vj+cQ7nH6n3/ZOKU1TyqTQrmUWFXPPVF6QVlNW1PbeHsfzfPx45hC4lr0yZ7raefSEE1/eN482tm+vs0dSEYFbfOLfE+vvJE7ywcR27UlMqXR8X2YZ5w0cx2oGC8DO792TJoQNsOJVY42MWwCXde3BBbPsGRK0orqPqfDqPmvPpoEHBAw0nnhbNg96B7imY/Fv6GkQDxgc0oV4aSvPy3x3bSCvIr3VYW5eSH48cYltyUiNGVtWsvnF4mz1qfQPWhMDXw4Pr+zb+Pu5f7d/L3O++Zs95iSfAnrRU5n73NV8f2Gf4eB4mE+/MmMmULl2BsuH1cpooe6ea3a8/z104VU33UZquJrLg6PXXX6dDhw54eXkxbNgwNm/eXGPbhQsXIoSodPHy8mrIs+AUqufTQUNCBvPJyc8otBfWmoRqaFwQNsotdTNPFiTwe/o6h6YHnK+DTzsnRuSYfFs+WzK3kV2ajcVkoXdALzr4qt4QpWaldjuf7tllaD6lSQj+t3sng6PbNkJk1Wvj588Hl17O3O++psRmqxK3SQi8zB58cOkVRPg27j7uh85k8OiKpWUlDau5vbzn8pHlv9I3IpJuBofhvT08eG3qxRwbnskX+/YQn5mJSQh6hodzTe++RPsHOO9BKEoL9cUXXzBv3jzeeusthg0bxksvvcTkyZM5dOgQERER1d4nICCAQ4cOVfzcFL7gqeTTQRbNg7u63M4Lh14GqDbB09CI8ArnypjLGzs80kvS+dfB57Bjr9f9BYLOfp2I8YlxcmR1K7GX8GnCF6zNWItd2tGECSkli/iKDj7tmdPhBjr5dWz0uJSm73h2FjklJYba2qVkw6kEh44fn3mGT/fuZm9qKrqUdA8L47o+/Rq0gGlwdFt+uv5GPti5nUX79lJkswJlq8Ov7t2Xm+IGEhsYWO/j19dHu3YYGjMRwP927+Tv4yc5dPxOwSHMv2BsvWJTFLdyRWkkB4/3wgsvcNttt3HTTTcB8NZbb/Hjjz/y/vvv8+ijj1Z7HyEEbdo0ra15VfJZD30D+/CXHvN4/9hC0kszMAkTUFa/T0cnLqgvt3a8GV+zb6PHtuT0Dw0q7yQQbkmaS+wlPHvweY4VHK9I6O3yjwQ6oTCBZw78i0d6/Jlu/l0bPT6laSu1O/Zly2o3tgivxGbjkRW/suTQwUoLmHalJvPZ3t2Ma9+Bl6fMwN/TsZ3EyrULDOKJsRN4ZNRokvPL5qpG+fnhZXasPJGz6FLyzYH9hgvCf3NgH0+Nm6hKIylKA+XmVt7a2tPTE8/z3ldKS0vZtm0b8+fPr7hO0zQmTZrEhg0bajx2fn4+7du3R9d1Bg4cyD//+U969+7t3AfgIJV81lOvgJ78O24B+3MPsC9nPyV6KUGWQEaEDiPcM9wtMRXYCll/ZqPDq9uBii1C7+p8Oz0Derggutp9l/R9pcTzfDoSKe28cuR1Xur/PGZNvXSVP7Tx80NgbKc6AbQNqHuIV5eS+37+gZUnjgHVL2D6PeEkNy35hk8uu6pBK+i9zB5NYj/z/NJSSuzGa4oW2WwUlJbWO/lWlGbFhT2fsbGxla5+4oknePLJJytdl5GRgd1uJzKy8ohLZGQkBw8erPbw3bt35/3336dfv37k5OTw/PPPM3LkSPbt20dMTOOPcJZTn+ANoAmNPoG96RPo3m8Q5RILE7FJx4tR+5l8GR85jvHhYwn1DHVBZLUr1UtZmba6zjmqEkmeLY+tWdsYHjqskaJTmoNwH1/Gtu/I7wkn6uy1k8B1fepexLPq+DGWHz9aaxu7lGxPTuLbg/u5xsAxmzpPk8nh+7izDJSitBSJiYkEnPOl+Pxez/oaMWIEI0aMqPh55MiR9OzZk7fffpu///3vTjlHfaglzS3IucPUjri07SVcGXO5WxJPgAO5BymyFxlqKxBsOrPFxREpzdHtg4bUmXhqQhDs5cWl3XvWebyPdu+otCq7JgJYuGsHsgWUTPE0mxnYJtrQMLomBIOj2uJRj4RVUZol3UUXyhYFnXupLvkMCwvDZDKRmppa6frU1FTDczo9PDwYMGAA8fHxjjxyp1PJZwtSn+F+gWBV2mrnB+OAfFu+4bYSSa4tt+6GSqszPCaWJ8dOAKg2eTIJgY9H2QpyP4ul1mNJKVmfmGBo7qOkbIV4rsEFT03dnP4DDO2opEvJjaogvKI0GovFwqBBg1ixYkXFdbqus2LFikq9m7Wx2+3s2bOHqKgoV4VpiEo+W5AIr3C6+XV1qL6nRJJResaFUdXNkXJUAoGvqfEXcinNw41xA/jw0isYcl4ZJQ9N47IevVhy7Wz6RdbdQ2CX0uFtMIub8f7r55rapRuj27WvtfdTE4LR7doztUu3RoxMUdyrvMi8sy+OmDdvHu+++y4ffvghBw4c4K677qKgoKBi9fuNN95YaUHS008/zdKlSzl27Bjbt29n9uzZnDx5kltvvdWpz42j1GSdFubi6On85/BLDt2nfLW+u/QM6IFF86BUt9bZViIZGDygEaJSmqvR7Tswun0HEnKyScjJwUPT6BEWTqADhZXNmkaQlxfZBrfA9DjbviUwaxpvTb+UPy/7mZ/jj1Ra5V/+7ymdu/L8RVMwaar/QmlFmkCppWuuuYb09HQef/xxUlJS6N+/P7/88kvFIqSEhAS0c/4us7KyuO2220hJSSE4OJhBgwaxfv16evVyzwY45YRsCROVDMrNzSUwMJCcnJxKE3tbmp+Tf+XzxEWG2mpo9A3sw7zuD7g4qtp9dOJjVqX9VudKfS/Ni1cGvICnSa2uVVxrwdrfeH/Htjp7QE1CcEn3nvznoqmNFFnjOXwmg8/27ubg2b3de4aFc12fOLqGumd+uNJ6ufPzu/zck7o+iNnJnz02ewnLj7zY4vOS86mezxZoatRkEgoTWX+m5rpf5XR0JkVOaISoand5zEz25uwjvXwSwtQAAB7lSURBVCSjxgRUILi98y0q8WwmMosKOVNYhI+HB9H+/k1iVw1HzOobx8Kd29Fl7XUYdCmZE9cye+O7hYbxxFj3vz8oSpOgSxBO7q/TW03/XyUq+WyhZrW7lkN5h8gszaqxhJGGoFdAryZRKsrP7Mffej3G20ffZW/uPrSz05EFAjt2Aj0CubnDHPoHx7k5UqUuq08c5/0dW1mb+McuQh2CgpgbN5BrevdtUD3MxtQuMIgXJ0/n/l9+QECVRTgaZQtV/z5+kqF5pIqiKEoZNezegqWXZPD8oRdIKU5FQ6voUSz/d7/Avtzb5a4m15OYVJTM+jMbyCnNwUOz0DuwJ/2D4tw+N1Wp2/Pr1/LG1k2V5gkCFUvgBkRFs9DAavOmZPPpU7y0cT0bTydWur5fZBseGDaC8R06uSmy5q3IamX58aOk5ufjZTYzIiaWziFqOF+pXpMYdu/0gGuG3Y+93GryknIq+WzhbLqNHdk7WZm2muSiFDSh0cWvMxMjx5etjG9mQ6FK07Vo3x4eXbG01jaaEEzs2Im3Z8xsnKCc6FhWJgcz0pESOoeE0CPMPTuZNXeldjsvblzH/3bvpNBqRRMCeXZqw7C2MTw+dgI91XOrnEclny1L8xj/UurNrJkZEjKYISGD3R2K0oLpUvLq5o2G2i07dpT4zDN0aWa9XJ2CQ+gUHOLuMJo1q93Ord8vZl3CyYrJQOdOZ9iadJorF33KJ5dfTf827q1DqChVuWC1u6FNgVseVSdDUZQG23gqkdN5xor/m4Rg0b49Lo5IaYre3ralUuJ5PruUlNjt3P7Dt5Ta67djm6IoTZ/q+VSUFsqqW9mauY0d2bsoshfhZ/ZjWOgQ+gX2RRPO/d55PDvLcFu7lA61V1oGq93Owl3b6+zn0aUko7CQpUePMKNbj0aJTVEMaQJ1PlsKlXwqSgu0K3sPbx99lwJ7AQKBRKKhsf7MBkItoTzQ9R7a+7Z32vmM7IFeToAqTt6CyLNfJjKLivCzWOgaElrt73dz0ikyi4oMHVMTgu8OHVTJp9K06BKnD5OrUkuKorQEe7L38uLhlyt+Li+1VV7tIKs0i2cOPMv/9XqMWJ8Yp5zTkVJDQgj6RqjSRM2dLiVf7d/L+zu3c/hMRsX1UX7+3BjXn7lxAyuV1TpTWOjQsdML8p0ar6IoTYfqflCUFsQu7bx3/H2AGuu76uhYdSsfnfjYaeftFR5Bv4hIQ28oAri6Vx+nnVtpfHZd56GlP/HoiqUcOSfxBEjOz+Pf69Zy/TeLKCgtrbje18Ox8lr+nk2rBJyiIHXXXFohlXwqSguyK3s32dacGhPPcjo6h/OPcLooyWnnfnjUGBCCugbgbxs4hHBfX6edV2l8b2zdxHeHDgLVD0JKJLtSU3hk+a8V1w1tG4OnyVitXgFM7NjZCZEqitIUqeRTUVqQ3Tl7MRn8sxYI9uTsddq5R8a245UpMzBrGtp5c0DL54Te0DeOP4+8wGnnVBpfic3Ge9u31dlOl5Kf4w+TmJMDlPVkXtGrj6H5wRaTict79mpwrIriVOULjpx9aYVU8qkoLUiJvcTwdHiBoMRe4tTzT+vajdVzbuWeIcOI9vPH02Qi0NOLS7r35Jurr+ep8ZOqJKZK87L82FHySo29bjQh+OrAH19wHhw2kjZ+/nUmoP+YcCEBnl4NilNRlKZLLThSlBYkyCPQcFsdnUAP5++oEeXvz4PDR/Hg8FEU2YtYn7GB39J/592kXzAnm+kd0JOJkRPo5NtR7bDVDCXkZlfZPrU25T2fAKE+Pnx11XXc+/P3bEtOwiw07FKiibISXP4WC0+Pn8Sl3Xu6KnxFqT+12t1pVPKpKC3I8NBh/JTyi6G2JmFicPAgl8VyNP8Y/zn0EgX2gkrXbziziXVnNjAqdAQ3d5yLWVNvQ82Jh2Zy6OPX47x5npF+fnx51XXsTUvlm4P7K/Z2Hxnbjuldu+Fl9nBuwIqiNDnqXV9RWpD2vu3o6teFo/nHKkorVUdDMCp0JH4efi6JI7kohWcPPo9Vt1a5rTyu9Wc2YhJmbuk01yUxKK4xMCq60paYtbFLycAatsnsExFJn4hIZ4amKK6lisw7jZrzqSgtzN1d7iDQIwCthj9vDUGsTyzXt7/GZTF8e/o7rLq11gRYIlmT8TtJTlxxr7jegDZRdA0JrbOqAYCPhwcXqyF0paWQuGDBkbsflHuo5FNRWpgQSwhP9v4/hoUOxXTeNpoWzcLEyAk81vMRvE3eLjl/rjWPzZlba008y2lorExb7ZI4mqqDGem8uHEdT6xewXPrf2dXSjKyGfV+CCH4vzHjEQbKaj08cjQ+HmoYXVGUytSwu6K0QEGWIO7sfBvXt7uWvTn7KLIX4e/hR9/APi5LOsudLDhpKPGEsiH4Q3mHXRpPU5GYk8NDS39ma/JpTEKULbaS8ObWzfQKC+f5i6bSIyzc3WEackG79rw29WL+9OuPWO32Sp03JiHQpeThUaO5MW6A22JUFKdTw+5Oo5JPRWnBAjz8GRk2vFHPacfuWHvpWPvm6FRuDpct+oSc4mKgbC7kuR86h85kcOWXn/HVVdc1mwR0SpeurIu+nS/37+Xbg/s5c3Zv98ldunJ9n360Cwxyd4iKojRRKvlUFMWpIjyNJ08aGpGeLX/RyWMrl5FTXFxjeSK7lJTYbPzp15/4+fobm00JqlAfH+4cPJQ7Bw91dyiK4nq6DgZHdRw7Zuuj5nwqiuJU0d7RZTU8DSxJ0dEZFzG2EaJynxPZWaxNOFlnXUy7lBw+k8GOlORGikxRFMU9VPKpKIrTXRw9o8795TU02npH0zewdyNF5R7Ljx01vKuTWWj8Et865sAqSrOjttd0GpV8KoridAOD+3NN7FUA1ZZ80tAIsYTwULc/oYmW/TaUW1JifEtRUdZeqVuJzcbvCSf44fBB1pw8QbGtak1ZRVGapmYx5/PEiRP8/e9/Z+XKlaSkpBAdHc3s2bP561//isVicXd4iqJUY1rUFGJ9Yvgl+Vf25u6vuN7X5MuEyHFMibzIZUXum5IAT0/DRdmREOSl9jSvTbHNymubN/HJnp3knJOo+1ksXNu7L/cPG4mf+lxQXEGtdneaZpF8Hjx4EF3Xefvtt+nSpQt79+7ltttuo6CggOeff97d4SmKUoO+gX3oG9iHrNIsskqzMWtmor2iWtWWmpM7d2XB2t8MtbVJnaldurk4ouar0Gpl9jeL2J2WWiWhzy8t5f2d2/k94SRfXHkNAZ4qiVecTO3t7jTN4hNgypQpTJkypeLnTp06cejQId58802VfCpKMxBsCSbYEuzuMNwiNjCQcR06subkiVoXHZmEoHtoGP0i2zRidM3LU7+trDbxLKdLSXzmGR5ZvpQ3p1/SyNEpimJUs51slZOTQ0hISK1tSkpKyM3NrXRRFEVpbM9MuJBQbx9MNcz9NAmBj4cHL0+Z3mzKLDW2jMJCFh/cX+cUBruULD16hFO5OY0UmdJaSKm75NIaNcvkMz4+nldffZU77rij1nYLFiwgMDCw4hIbG9tIESqKovyhjZ8/i6+ZxcjYdgBoQmDWtIpkNC4yim+uvp7OIaHuDLNJ++nIIewGayJqQvDdoQMujkhRlPpy67D7o48+yrPPPltrmwMHDtCjR4+Kn0+fPs2UKVO46qqruO2222q97/z585k3b17Fz7m5uSoBVRTFLaL8/flw5pUcz87il/jDZBcX42/xZFKnzs1mVyN3SsnPx6Rp2AwkoEIIUvLzGyEqpVWR0vlzNNWCo8b30EMPMXfu3FrbdOrUqeLfSUlJjB8/npEjR/LOO+/UeXxPT088PT0bGqaiKIrTdAwK5q7Bw9wdRrPjaTYhHfig9jI3iyUNitIqufWvMzw8nPBwY9/4T58+zfjx4xk0aBAffPABmtYsZwwoiqIo9TAiph0vb9pgqK1N1xkeo0a5FCeTLljtrno+m67Tp08zbtw42rdvz/PPP096enrFbW3aqJWhiqIoLd2Q6LZ0Dg7heFZmrbtrCyDSz49x7Ts2VmiKojioWSSfy5YtIz4+nvj4eGJiYird5sgwjKIoitI8CSH4x/hJzF78JUJWv3lreZ2Af4y/EJMaHVOcTddBOHl1ulrt3nTNnTsXKWW1F0VRFKV1GBYTy/uXXl6xg5E4m26WJ53eHh68Of0SJnTsVMMRFKUB1N7uTtMsej4VRVEUBWB0uw5svOVOfjhyiB8PHyKzqJAgL28md+nKzO498VVbaypKk6eST0VRFKVZ8fbw4KpefbiqVx93h6K0IlLXkU4edldF5hVFURRFURTFxVTPp6IoiqIoSl1UqSWnUT2fiqIoiqIoSqNRPZ+KoiiKoih10SUI1fPpDKrnU1EURVEURWk0qudTURRFURSlLlJCrftr1feYrY/q+VQURVEURVEajer5VBRFURRFqYPUJdLJcz5b606NKvlUFEVRFEWpi9Rx/rC7KjKvKIqiKIqiKC6lkk9FURRFUZQ6SF265OKo119/nQ4dOuDl5cWwYcPYvHlzre2//PJLevTogZeXF3379uWnn36q71PgNCr5VBRFURRFaQa++OIL5s2bxxNPPMH27duJi4tj8uTJpKWlVdt+/fr1XHfdddxyyy3s2LGDmTNnMnPmTPbu3dvIkVcmZCua7Zqbm0tgYCA5OTkEBAS4OxxFURRFUQxw5+d3+bnHcSlm4eHUY9ukldV8Z/hxDRs2jCFDhvDaa68BoOs6sbGx3HfffTz66KNV2l9zzTUUFBTwww8/VFw3fPhw+vfvz1tvveW8B+KgVrXgqDzPzs3NdXMkiqIoiqIYVf657c7+MhtWp2/tbsMKVM1LPD098fT0rHRdaWkp27ZtY/78+RXXaZrGpEmT2LBhQ7XH37BhA/Pmzat03eTJk/n222+dEH39tarkMy8vD4DY2Fg3R6IoiqIoiqPy8vIIDAxs1HNaLBbatGnD2hTXzJX08/Orkpc88cQTPPnkk5Wuy8jIwG63ExkZWen6yMhIDh48WO2xU1JSqm2fkpLS8MAboFUln9HR0SQmJuLv748Qwt3hkJubS2xsLImJiWoaQCNQz3fjUc9141LPd+NRz3XjKn++ExISEEIQHR3d6DF4eXlx/PhxSktLXXJ8KWWVnOT8Xs+WplUln5qmERMT4+4wqggICFBvYo1IPd+NRz3XjUs9341HPdeNKzAw0K3Pt5eXF15eXm47P0BYWBgmk4nU1NRK16emptKmTZtq79OmTRuH2jcWtdpdURRFURSlibNYLAwaNIgVK1ZUXKfrOitWrGDEiBHV3mfEiBGV2gMsW7asxvaNpVX1fCqKoiiKojRX8+bNY86cOQwePJihQ4fy0ksvUVBQwE033QTAjTfeSNu2bVmwYAEADzzwAGPHjuU///kP06dP5/PPP2fr1q2888477nwYKvl0J09PT5544okWP7ejqVDPd+NRz3XjUs9341HPdeNSz3dl11xzDenp6Tz++OOkpKTQv39/fvnll4pFRQkJCWjaH4PaI0eO5NNPP+Vvf/sbjz32GF27duXbb7+lT58+7noIQCur86koiqIoiqK4l5rzqSiKoiiKojQalXwqiqIoiqIojUYln4qiKIqiKEqjUcmnoiiKoiiK0mhU8tkEnDhxgltuuYWOHTvi7e1N586deeKJJ1y2m4ICzzzzDCNHjsTHx4egoCB3h9PivP7663To0AEvLy+GDRvG5s2b3R1Si7RmzRouvvhioqOjEUK4fb/mlmzBggUMGTIEf39/IiIimDlzJocOHXJ3WC3Wm2++Sb9+/SqK+Y8YMYKff/7Z3WEpTqKSzybg4MGD6LrO22+/zb59+3jxxRd56623eOyxx9wdWotVWlrKVVddxV133eXuUFqcL774gnnz5vHEE0+wfft24uLimDx5Mmlpae4OrcUpKCggLi6O119/3d2htHi//fYb99xzDxs3bmTZsmVYrVYuuugiCgoK3B1aixQTE8O//vUvtm3bxtatW5kwYQKXXnop+/btc3doihOoUktN1HPPPcebb77JsWPH3B1Ki7Zw4UL+9Kc/kZ2d7e5QWoxhw4YxZMgQXnvtNaBsB47Y2Fjuu+8+Hn30UTdH13IJIVi8eDEzZ850dyitQnp6OhEREfz222+MGTPG3eG0CiEhITz33HPccsst7g5FaSDV89lE5eTkEBIS4u4wFMUhpaWlbNu2jUmTJlVcp2kakyZNYsOGDW6MTFGcKycnB0C9TzcCu93O559/TkFBgdu3hVScQ+1w1ATFx8fz6quv8vzzz7s7FEVxSEZGBna7vWK3jXKRkZEcPHjQTVEpinPpus6f/vQnRo0a5fadYlqyPXv2MGLECIqLi/Hz82Px4sX06tXL3WEpTqB6Pl3o0UcfRQhR6+X8D+TTp08zZcoUrrrqKm677TY3Rd481ef5VhRFcdQ999zD3r17+fzzz90dSovWvXt3du7cyaZNm7jrrruYM2cO+/fvd3dYihOonk8Xeuihh5g7d26tbTp16lTx76SkJMaPH8/IkSN55513XBxdy+Po8604X1hYGCaTidTU1ErXp6am0qZNGzdFpSjOc++99/LDDz+wZs0aYmJi3B1Oi2axWOjSpQsAgwYNYsuWLbz88su8/fbbbo5MaSiVfLpQeHg44eHhhtqePn2a8ePHM2jQID744AM0TXVKO8qR51txDYvFwqBBg1ixYkXFwhdd11mxYgX33nuve4NTlAaQUnLfffexePFiVq9eTceOHd0dUquj6zolJSXuDkNxApV8NgGnT59m3LhxtG/fnueff5709PSK21RvkWskJCSQmZlJQkICdrudnTt3AtClSxf8/PzcG1wzN2/ePObMmcPgwYMZOnQoL730EgUFBdx0003uDq3Fyc/PJz4+vuLn48ePs3PnTkJCQmjXrp0bI2t57rnnHj799FO+++47/P39SUlJASAwMBBvb283R9fyzJ8/n6lTp9KuXTvy8vL49NNPWb16Nb/++qu7Q1OcQSpu98EHH0ig2oviGnPmzKn2+V61apW7Q2sRXn31VdmuXTtpsVjk0KFD5caNG90dUou0atWqal/Hc+bMcXdoLU5N79EffPCBu0NrkW6++WbZvn17abFYZHh4uJw4caJcunSpu8NSnETV+VQURVEURVEajZpYqCiKoiiKojQalXwqiqIoiqIojUYln4qiKIqiKEqjUcmnoiiKovx/e/caE8XZxQH8v0UXcWdBYRewVZZGvCwWq6ItFhX9wKU2immC12q3GhsFlcRrarCliyZ416ihVZtALVFb01SLGmMRIqFa0ApiwlUltAQ1KrauVgQ5/WCcOIK49uVdUP+/ZBNmnsuceTaQw5mZXSJyGSafREREROQyTD6JiIiIyGWYfBIRERGRyzD5JCIiIiKXYfJJ9BIJDAzEli1b2m0+m82mfkd7e8nNzYVOp8OtW7fadV4iInoxMPkk6oRsNht0Oh10Oh30ej2CgoJgt9vR1NTU5rjCwkJ8+umn7RbH1q1bkZ6e3m7zPY9z584hLi4Ofn5+6NatG/r164e5c+eioqKiQ+LprJz9h2Pnzp0YO3YsPD09mfwTUYdi8knUScXExKCurg6VlZVYsmQJkpOTsX79+lb73r9/HwBgNpvRvXv3dovBy8sLPXr0aLf5nJWVlYWwsDA0NDQgMzMTpaWl+O677+Dl5YVVq1a5PJ6Xwd27dxETE4OVK1d2dChE9Krr6C+XJ6KWPv74Y4mNjdXsi4yMlLCwME376tWrpVevXhIYGCgiIhaLRTZv3qyOASC7du2SSZMmiYeHhwQFBcnBgwc18164cEE++OADMRqNoiiKjBo1SqqqqlqNIyIiQhISEiQhIUE8PT3Fx8dHkpKSpLm5We3z7bffSmhoqCiKIn5+fjJt2jS5evWq2p6TkyMApL6+vtVzv3PnjphMJpk0aVKr7Y+Py83NlREjRoherxd/f39ZsWKFNDY2auJdsGCBJCYmSo8ePcTX11d27twpDodDbDabKIoiffv2lSNHjrSILysrS0JCQsTd3V3effddKSkp0cRx4MABCQ4OFr1eLxaLRTZs2KBpt1gssmbNGvnkk09EURTp06ePfP3115o+NTU1EhcXJ15eXtKzZ0+ZOHGiXL58WW1/tP7r168Xf39/8fb2lvj4eLl//756fgA0r2d51voTEf2/sfJJ9ILw8PBQK5wAkJ2djfLychw/fhxZWVlPHffll19i8uTJOH/+PMaPH48ZM2bg5s2bAIDa2lqMGTMG7u7uOHHiBM6ePYvZs2e3eXk/IyMDXbp0QUFBAbZu3YpNmzZh9+7dantjYyNSUlJQXFyMn376CdXV1bDZbE6f57Fjx3D9+nUsX7681fZHldja2lqMHz8eI0aMQHFxMdLS0vDNN99g9erVLeI1mUwoKCjAwoULMX/+fMTFxeG9997D77//jqioKMycORN3797VjFu2bBk2btyIwsJCmM1mTJgwAY2NjQCAs2fPYvLkyZg6dSpKSkqQnJyMVatWtbhFYePGjRg+fDjOnTuH+Ph4zJ8/H+Xl5eo6RUdHw2g0Ii8vD/n5+VAUBTExMZr3OScnBxcvXkROTg4yMjKQnp6uHufHH39E7969YbfbUVdXh7q6OqfXmYiow3R09ktELT1ecWxubpbjx4+Lu7u7LF26VG338/OThoYGzbjWKp9JSUnqtsPhEABy9OhRERH57LPP5M0331QraW3FIfKw0ma1WjWVzhUrVojVan3quRQWFgoAuX37tog8u/K2du1aASA3b9586pwiIitXrpQBAwZoYtmxY4coiiIPHjxQ4x01apTa3tTUJAaDQWbOnKnuq6urEwBy6tQpTXz79u1T+9y4cUM8PDxk//79IiIyffp0iYyM1MSzbNkyCQ4OVrctFot89NFH6nZzc7P4+vpKWlqaiIjs2bOnRfwNDQ3i4eEhx44dE5GH62+xWKSpqUntExcXJ1OmTNEc5/H3/FlY+SSijsbKJ1EnlZWVBUVR0K1bN7z//vuYMmUKkpOT1faQkBDo9fpnzjN48GD1Z4PBAE9PT1y7dg0AUFRUhNGjR6Nr165OxxUWFgadTqdujxw5EpWVlXjw4AGAh1XBCRMmICAgAEajEREREQCAmpoap+YXEaf6lZaWYuTIkZpYwsPD4XA48Oeff6r7Hj9/Nzc3+Pj4ICQkRN3n5+cHAOqaPH5ej3h7e2PAgAEoLS1Vjx0eHq7pHx4erlmHJ4+t0+ng7++vHqe4uBhVVVUwGo1QFAWKosDb2xv37t3DxYsX1XGDBg2Cm5ubut2rV68WsRIRvUi6dHQARNS6cePGIS0tDXq9Hq+//jq6dNH+uhoMBqfmeTKx1Ol0aG5uBvDwUn57unPnDqKjoxEdHY3MzEyYzWbU1NQgOjpacym5Lf379wcAlJWVaRLA/6q1839836Pk9dGatKe21t7hcCA0NBSZmZktxpnNZqfmICJ6EbHySdRJGQwGBAUFISAgoEXi2V4GDx6MvLw89V5GZ/z222+a7dOnT6Nfv35wc3NDWVkZbty4gdTUVIwePRoDBw587ipdVFQUTCYT1q1b12r7o48IslqtOHXqlKZSmp+fD6PRiN69ez/XMVtz+vRp9ef6+npUVFTAarWqx87Pz9f0z8/PR//+/TVVyrYMGzYMlZWV8PX1RVBQkObl5eXldJx6vV5TbSUi6uyYfBK9whYsWIC///4bU6dOxZkzZ1BZWYk9e/aoD8W0pqamBosXL0Z5eTn27t2Lbdu2ITExEQAQEBAAvV6Pbdu24dKlSzh06BBSUlKeKyaDwYDdu3fj8OHDmDhxIn755RdUV1fjzJkzWL58OebNmwcAiI+Pxx9//IGFCxeirKwMBw8exBdffIHFixfjtdf+9z9tdrsd2dnZuHDhAmw2G0wmk/qB+0uWLEF2djZSUlJQUVGBjIwMbN++HUuXLnV6/hkzZsBkMiE2NhZ5eXm4fPkycnNzsWjRIs1tA88SGBiIkydPora2FtevX39qvytXrqCoqAhVVVUAgJKSEhQVFakPnxERuQqTT6JXmI+PD06cOAGHw4GIiAiEhoZi165dbd4DOmvWLPzzzz945513kJCQgMTERPWD7c1mM9LT0/HDDz8gODgYqamp2LBhw3PHFRsbi19//RVdu3bF9OnTMXDgQEybNg1//fWX+jT7G2+8gSNHjqCgoABvv/025s2bhzlz5iApKem/LcYTUlNTkZiYiNDQUFy5cgU///yzeo/tsGHD8P3332Pfvn1466238Pnnn8Nutz/XU/3du3fHyZMnERAQgA8//BBWqxVz5szBvXv34Onp6fQ8drsd1dXV6Nu3r+Zy/ZO++uorDB06FHPnzgUAjBkzBkOHDsWhQ4ecPhYRUXvQibN39xPRK2/s2LEYMmRIu36FZ2eTm5uLcePGob6+vkM+YJ+I6GXHyicRERERuQyTTyIiIiJyGV52JyIiIiKXYeWTiIiIiFyGyScRERERuQyTTyIiIiJyGSafREREROQyTD6JiIiIyGWYfBIRERGRyzD5JCIiIiKXYfJJRERERC7zL2JAyBZAO4d3AAAAAElFTkSuQmCC\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "plt.figure(figsize=(8,6))\n",
        "\n",
        "for i in sorted(pca_df[\"Cluster\"].unique()):\n",
        "    temp = pca_df[pca_df[\"Cluster\"] == i]\n",
        "\n",
        "    plt.scatter(\n",
        "        temp[\"PC1\"],\n",
        "        temp[\"PC2\"],\n",
        "        label=f\"Cluster {i}\"\n",
        "    )\n",
        "\n",
        "plt.title(\"PCA Visualization with Cluster Labels\")\n",
        "plt.xlabel(\"Principal Component 1\")\n",
        "plt.ylabel(\"Principal Component 2\")\n",
        "plt.legend()\n",
        "plt.grid(True)\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 564
        },
        "id": "qidA38FSAb8Q",
        "outputId": "a0593774-32da-4aa1-961b-dc7e07f4a05f"
      },
      "execution_count": 22,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 800x600 with 1 Axes>"
            ],
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAArMAAAIjCAYAAAAQgZNYAAAAOnRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjEwLjAsIGh0dHBzOi8vbWF0cGxvdGxpYi5vcmcvlHJYcgAAAAlwSFlzAAAPYQAAD2EBqD+naQAArZJJREFUeJzs3Xl8E3X6B/DP9ErPtAV6gbW0UMBSQCiigIgIlAqCi3iLgteuiAeCv+XYVQ6FWg88EVFXxEUQV1BRsJRTYEUKlKrIIWA5FnpxtIWWXsn8/ggJTXNN0skxyef9evHSTiYz32RIefKd5/s8giiKIoiIiIiIFMjP3QMgIiIiInIUg1kiIiIiUiwGs0RERESkWAxmiYiIiEixGMwSERERkWIxmCUiIiIixWIwS0RERESKxWCWiIiIiBSLwSwRERERKRaDWSKy6dixYxAEAZ9++qnHjWPWrFkQBMHlY3HXeR316aefQhAEHDt2TPK+u3fvdv7AAAiCgFmzZrnkXEokCAKeeuop2Y7nKZ9nIrkwmCVyMn1goP8THByMTp064amnnkJpaanJ/qWlpXj++efRpUsXhIaGIiwsDBkZGXj55ZdRUVFh9hx9+vSBIAhYuHChpDHNnz8fgiBgw4YNFvf56KOPIAgCVq9eLemY3qimpgazZs3Cli1b3D0Up3j//fedGtAUFhZi7NixSExMhEqlQqtWrTBkyBAsXrwYGo3Gaedt6vTp05g1axYKCwtdcj4A2LJlCwRBwFdffeWycxL5MgazRC4yZ84c/Pvf/8Z7772Hfv36YeHChejbty9qamoM++zatQvp6elYsGABBgwYgPnz5+ONN95Az5498corr+Duu+82Oe7hw4exa9cutG/fHp9//rmksdx7773w8/PDsmXLLO6zbNkytG7dGrfeeiuSkpJw6dIlPPjgg/a/cCf75z//iUuXLjnl2DU1NZg9e7bZYNaZ53WGBx98EJcuXUJSUpJhmzOD2Y8//hi9e/fG5s2b8cADD+D999/Hiy++iJCQEDz66KPIyclxynmbO336NGbPnu3SYJaIXCvA3QMg8hW33norevfuDQB47LHH0Lp1a8yfPx/ffvst7rvvPlRUVGD06NHw9/fH3r170aVLF6Pnz507Fx999JHJcZcuXYrY2Fi88cYbuPPOO3Hs2DG0b9/e6ljatm2LQYMGYdWqVVi4cCFUKpXR46dOncLWrVvx17/+FYGBgQCA4ODgFrx65wkICEBAgOt/lbnrvI7y9/eHv7+/S871888/44knnkDfvn2xdu1aREREGB6bNGkSdu/ejX379rlkLM5SXV2NsLAwdw+DiMCZWSK3ueWWWwAARUVFAIBFixbh1KlTmD9/vkkgCwBxcXH45z//abJ92bJluPPOO3HbbbchMjLS6mxrU2PHjkVlZSXWrFlj8tgXX3wBrVaLBx54AID5HLuSkhI8/PDDuOqqq6BSqZCQkIDbb7/dKCfTUi5k+/btMX78eMPP586dw/PPP49u3bohPDwcarUat956K3755Rebr6N57ur48eON0jqa/tGPpb6+Hi+++CIyMjIQGRmJsLAwDBgwAJs3bzYc59ixY4iJiQEAzJ492+QY5nJmGxsb8dJLL6FDhw5QqVRo3749ZsyYgbq6OpPXf9ttt2H79u3o06cPgoODkZKSgs8++8zm6+3VqxfuuOMOo23dunWDIAj49ddfDdtWrFgBQRBw4MABAKY5s+3bt8fvv/+OH3/80fDabr75ZqPj1tXVYfLkyYiJiUFYWBhGjx6N8vJym2PUv1+ff/65USCr17t3b6Pr39z48ePNfiEz956vX78eN954I6KiohAeHo7OnTtjxowZAHS3+6+77joAwMMPP2x4nU3/Hu/cuRNZWVmIjIxEaGgoBg4ciP/+979mz7t//37cf//9iI6Oxo033mjzfbDl9ddfR79+/dC6dWuEhIQgIyPDamrC559/js6dOyM4OBgZGRnYunWryT6nTp3CI488gri4OKhUKnTt2hWffPKJzbFI+TwTeSrlTCsQeZmjR48CAFq3bg0AWL16NUJCQnDnnXdKPsbOnTtx5MgRLF68GEFBQbjjjjvw+eefG/4xt+aOO+7AhAkTsGzZMpPgaNmyZUhKSkL//v0tPn/MmDH4/fff8fTTT6N9+/YoKyvD+vXrceLECZszw839+eef+Oabb3DXXXchOTkZpaWlWLRoEQYOHIj9+/ejbdu2ko/1t7/9DUOGDDHalpubi88//xyxsbEAgKqqKnz88ce477778Pjjj+PChQv417/+hWHDhiE/Px/XXnstYmJisHDhQkyYMAGjR482vEfdu3e3eO7HHnsMS5YswZ133okpU6Zg586dyM7OxoEDB/D1118b7XvkyBHceeed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PXnyJL799ltkZmYaagiPGTMGK1euNFvD19rrkfp5JvJUnJkl8hDR0dH4+uuvMXz4cFx77bUYO3asYcFHQUEBli9fjr59+wLQpRi0bt3aUGKouVGjRuGjjz7CmjVrTMpuNScIAu6//37MmzcPgK5TmS1//PEHBg8ejLvvvhtpaWkICAjA119/jdLSUtx7772G/R577DE88cQTGDNmDIYOHYpffvkF69atM5ltve222zBnzhw8/PDD6NevH3777Td8/vnnSElJsTmW5p555hmUl5fj73//O7744gujx7p3747u3bvjtttuw6pVqzB69GiMGDECRUVF+OCDD5CWlmYU2ISEhCAtLQ0rVqxAp06d0KpVK6SnpxtKITXVo0cPjBs3Dh9++CEqKiowcOBA5OfnY8mSJfjLX/4iaZZcqo4dOyI+Ph6HDh3C008/bdh+0003YerUqQAgKZjNyMjAwoUL8fLLL6Njx46IjY011D9uiX79+mHBggV48skn0aVLFzz44INITU3FhQsXsGXLFqxevdrwxcyce++9F1OnTsXo0aPxzDPPoKamBgsXLkSnTp2MAro5c+Zg69atGDFiBJKSklBWVob3338fV111laEObIcOHRAVFYUPPvgAERERCAsLw/XXX4/k5GR8/PHHuPXWW9G1a1c8/PDDaNeuHU6dOoXNmzdDrVbju+++a9H7sHLlShw8eNBk+7hx4zBixAjMnz8fWVlZuP/++1FWVoYFCxagY8eORkG8Xnp6OoYNG2ZUmgvQ1fTVe+WVV7B582Zcf/31ePzxx5GWloZz586hoKAAGzZsMBv8A9I/z0Qey42VFIh8gr7M0a5duyTtf/r0afG5554TO3XqJAYHB4uhoaFiRkaGOHfuXLGyslIsLS0VAwICxAcffNDiMWpqasTQ0FCjsj3W/P777yIAUaVSiefPnzd5vHkpnzNnzogTJ04Uu3TpIoaFhYmRkZHi9ddfL3755ZdGz9NoNOLUqVPFNm3aiKGhoeKwYcPEI0eOmC3NNWXKFDEhIUEMCQkR+/fvL+7YsUMcOHCgUekoKaW5Bg4caChl1PyPvvyTVqsV582bJyYlJYkqlUrs2bOn+P3335stCfXTTz+JGRkZYlBQkNExzJUEa2hoEGfPni0mJyeLgYGBYmJiojh9+nSxtrbWaL+kpCRxxIgRJu9z89drzV133SUC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\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "**Observations**"
      ],
      "metadata": {
        "id": "GHnZqomEBWuF"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Observations\n",
        "\n",
        "1. The Elbow Method indicates that **5 clusters** provide the optimal segmentation.\n",
        "\n",
        "2. PCA reduces the high-dimensional dataset into two principal components, making the clusters easier to visualize.\n",
        "\n",
        "3. The customer groups represent different spending patterns and income levels, helping businesses target specific customer segments through personalized marketing campaigns.\n",
        "\n",
        "4. High-income and high-spending customers are ideal targets for premium offers, while low-spending customers can be engaged through discounts and loyalty programs."
      ],
      "metadata": {
        "id": "lJRFpomdBbM1"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**# Task 5: Conclusion**"
      ],
      "metadata": {
        "id": "81ArtX81Bdu6"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Customer segmentation using K-Means clustering successfully divided customers into five meaningful groups based on their annual income and spending behavior. The Elbow Method identified the optimal number of clusters, while PCA reduced the multidimensional dataset into two principal components for easy visualization. These customer segments can help shopping malls design targeted marketing campaigns, improve customer satisfaction, and increase business revenue. One limitation of K-Means is that the number of clusters must be specified in advance and the algorithm is sensitive to outliers. An important advantage of PCA is that it simplifies complex datasets while preserving most of the important information, making cluster visualization and interpretation much easier."
      ],
      "metadata": {
        "id": "uAq-zb39Bhlh"
      }
    }
  ]
}