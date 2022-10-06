# IBM-Project-16871-1659624114
Car Resale value Prediction
{
  "cells": [
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "e0507835",
      "metadata": {
        "id": "e0507835"
      },
      "outputs": [],
      "source": [
        "import pandas as pd\n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "6231667d",
      "metadata": {
        "id": "6231667d"
      },
      "outputs": [],
      "source": [
        "import warnings\n",
        "warnings.filterwarnings('ignore')"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "from google.colab import files\n",
        "uploaded = files.upload()\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 74
        },
        "id": "hCbgYXZg3EL2",
        "outputId": "f70e8dd5-d078-465e-9394-e7d68c443c97"
      },
      "id": "hCbgYXZg3EL2",
      "execution_count": null,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<IPython.core.display.HTML object>"
            ],
            "text/html": [
              "\n",
              "     <input type=\"file\" id=\"files-f5905dc6-e190-4b47-a69c-dff69fbaa048\" name=\"files[]\" multiple disabled\n",
              "        style=\"border:none\" />\n",
              "     <output id=\"result-f5905dc6-e190-4b47-a69c-dff69fbaa048\">\n",
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
            "Saving abalone.csv to abalone.csv\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "7c34a0ae",
      "metadata": {
        "id": "7c34a0ae"
      },
      "outputs": [],
      "source": [
        "df=pd.read_csv('abalone.csv')"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "7bf7e62b",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "7bf7e62b",
        "outputId": "ff08faf1-0cf1-4920-f2b3-c767b34d8900"
      },
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "  Sex  Length  Diameter  Height  Whole weight  Shucked weight  Viscera weight  \\\n",
              "0   M   0.455     0.365   0.095        0.5140          0.2245          0.1010   \n",
              "1   M   0.350     0.265   0.090        0.2255          0.0995          0.0485   \n",
              "2   F   0.530     0.420   0.135        0.6770          0.2565          0.1415   \n",
              "3   M   0.440     0.365   0.125        0.5160          0.2155          0.1140   \n",
              "4   I   0.330     0.255   0.080        0.2050          0.0895          0.0395   \n",
              "\n",
              "   Shell weight  Rings  \n",
              "0         0.150     15  \n",
              "1         0.070      7  \n",
              "2         0.210      9  \n",
              "3         0.155     10  \n",
              "4         0.055      7  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-9cc3c108-340e-4804-b45f-52edb89c13c7\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Sex</th>\n",
              "      <th>Length</th>\n",
              "      <th>Diameter</th>\n",
              "      <th>Height</th>\n",
              "      <th>Whole weight</th>\n",
              "      <th>Shucked weight</th>\n",
              "      <th>Viscera weight</th>\n",
              "      <th>Shell weight</th>\n",
              "      <th>Rings</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>M</td>\n",
              "      <td>0.455</td>\n",
              "      <td>0.365</td>\n",
              "      <td>0.095</td>\n",
              "      <td>0.5140</td>\n",
              "      <td>0.2245</td>\n",
              "      <td>0.1010</td>\n",
              "      <td>0.150</td>\n",
              "      <td>15</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>M</td>\n",
              "      <td>0.350</td>\n",
              "      <td>0.265</td>\n",
              "      <td>0.090</td>\n",
              "      <td>0.2255</td>\n",
              "      <td>0.0995</td>\n",
              "      <td>0.0485</td>\n",
              "      <td>0.070</td>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>F</td>\n",
              "      <td>0.530</td>\n",
              "      <td>0.420</td>\n",
              "      <td>0.135</td>\n",
              "      <td>0.6770</td>\n",
              "      <td>0.2565</td>\n",
              "      <td>0.1415</td>\n",
              "      <td>0.210</td>\n",
              "      <td>9</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>M</td>\n",
              "      <td>0.440</td>\n",
              "      <td>0.365</td>\n",
              "      <td>0.125</td>\n",
              "      <td>0.5160</td>\n",
              "      <td>0.2155</td>\n",
              "      <td>0.1140</td>\n",
              "      <td>0.155</td>\n",
              "      <td>10</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>I</td>\n",
              "      <td>0.330</td>\n",
              "      <td>0.255</td>\n",
              "      <td>0.080</td>\n",
              "      <td>0.2050</td>\n",
              "      <td>0.0895</td>\n",
              "      <td>0.0395</td>\n",
              "      <td>0.055</td>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-9cc3c108-340e-4804-b45f-52edb89c13c7')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
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
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-9cc3c108-340e-4804-b45f-52edb89c13c7 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-9cc3c108-340e-4804-b45f-52edb89c13c7');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 5
        }
      ],
      "source": [
        "df.head()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "607f4b5d",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 300
        },
        "id": "607f4b5d",
        "outputId": "df3da18c-79d7-4776-ab43-d3dcb34fb036"
      },
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "            Length     Diameter       Height  Whole weight  Shucked weight  \\\n",
              "count  4177.000000  4177.000000  4177.000000   4177.000000     4177.000000   \n",
              "mean      0.523992     0.407881     0.139516      0.828742        0.359367   \n",
              "std       0.120093     0.099240     0.041827      0.490389        0.221963   \n",
              "min       0.075000     0.055000     0.000000      0.002000        0.001000   \n",
              "25%       0.450000     0.350000     0.115000      0.441500        0.186000   \n",
              "50%       0.545000     0.425000     0.140000      0.799500        0.336000   \n",
              "75%       0.615000     0.480000     0.165000      1.153000        0.502000   \n",
              "max       0.815000     0.650000     1.130000      2.825500        1.488000   \n",
              "\n",
              "       Viscera weight  Shell weight        Rings  \n",
              "count     4177.000000   4177.000000  4177.000000  \n",
              "mean         0.180594      0.238831     9.933684  \n",
              "std          0.109614      0.139203     3.224169  \n",
              "min          0.000500      0.001500     1.000000  \n",
              "25%          0.093500      0.130000     8.000000  \n",
              "50%          0.171000      0.234000     9.000000  \n",
              "75%          0.253000      0.329000    11.000000  \n",
              "max          0.760000      1.005000    29.000000  "
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-6df48eac-93d6-4b7b-9180-9dc48385eae6\">\n",
              "    <div class=\"colab-df-container\">\n",
              "      <div>\n",
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
              "      <th>Length</th>\n",
              "      <th>Diameter</th>\n",
              "      <th>Height</th>\n",
              "      <th>Whole weight</th>\n",
              "      <th>Shucked weight</th>\n",
              "      <th>Viscera weight</th>\n",
              "      <th>Shell weight</th>\n",
              "      <th>Rings</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "      <td>4177.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>0.523992</td>\n",
              "      <td>0.407881</td>\n",
              "      <td>0.139516</td>\n",
              "      <td>0.828742</td>\n",
              "      <td>0.359367</td>\n",
              "      <td>0.180594</td>\n",
              "      <td>0.238831</td>\n",
              "      <td>9.933684</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>0.120093</td>\n",
              "      <td>0.099240</td>\n",
              "      <td>0.041827</td>\n",
              "      <td>0.490389</td>\n",
              "      <td>0.221963</td>\n",
              "      <td>0.109614</td>\n",
              "      <td>0.139203</td>\n",
              "      <td>3.224169</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>0.075000</td>\n",
              "      <td>0.055000</td>\n",
              "      <td>0.000000</td>\n",
              "      <td>0.002000</td>\n",
              "      <td>0.001000</td>\n",
              "      <td>0.000500</td>\n",
              "      <td>0.001500</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>0.450000</td>\n",
              "      <td>0.350000</td>\n",
              "      <td>0.115000</td>\n",
              "      <td>0.441500</td>\n",
              "      <td>0.186000</td>\n",
              "      <td>0.093500</td>\n",
              "      <td>0.130000</td>\n",
              "      <td>8.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>0.545000</td>\n",
              "      <td>0.425000</td>\n",
              "      <td>0.140000</td>\n",
              "      <td>0.799500</td>\n",
              "      <td>0.336000</td>\n",
              "      <td>0.171000</td>\n",
              "      <td>0.234000</td>\n",
              "      <td>9.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>0.615000</td>\n",
              "      <td>0.480000</td>\n",
              "      <td>0.165000</td>\n",
              "      <td>1.153000</td>\n",
              "      <td>0.502000</td>\n",
              "      <td>0.253000</td>\n",
              "      <td>0.329000</td>\n",
              "      <td>11.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>0.815000</td>\n",
              "      <td>0.650000</td>\n",
              "      <td>1.130000</td>\n",
              "      <td>2.825500</td>\n",
              "      <td>1.488000</td>\n",
              "      <td>0.760000</td>\n",
              "      <td>1.005000</td>\n",
              "      <td>29.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "      <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-6df48eac-93d6-4b7b-9180-9dc48385eae6')\"\n",
              "              title=\"Convert this dataframe to an interactive table.\"\n",
              "              style=\"display:none;\">\n",
              "        \n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "       width=\"24px\">\n",
              "    <path d=\"M0 0h24v24H0V0z\" fill=\"none\"/>\n",
              "    <path d=\"M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z\"/><path d=\"M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z\"/>\n",
              "  </svg>\n",
              "      </button>\n",
              "      \n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      flex-wrap:wrap;\n",
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
              "      <script>\n",
              "        const buttonEl =\n",
              "          document.querySelector('#df-6df48eac-93d6-4b7b-9180-9dc48385eae6 button.colab-df-convert');\n",
              "        buttonEl.style.display =\n",
              "          google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "        async function convertToInteractive(key) {\n",
              "          const element = document.querySelector('#df-6df48eac-93d6-4b7b-9180-9dc48385eae6');\n",
              "          const dataTable =\n",
              "            await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                     [key], {});\n",
              "          if (!dataTable) return;\n",
              "\n",
              "          const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "            '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "            + ' to learn more about interactive tables.';\n",
              "          element.innerHTML = '';\n",
              "          dataTable['output_type'] = 'display_data';\n",
              "          await google.colab.output.renderOutput(dataTable, element);\n",
              "          const docLink = document.createElement('div');\n",
              "          docLink.innerHTML = docLinkHtml;\n",
              "          element.appendChild(docLink);\n",
              "        }\n",
              "      </script>\n",
              "    </div>\n",
              "  </div>\n",
              "  "
            ]
          },
          "metadata": {},
          "execution_count": 6
        }
      ],
      "source": [
        "df.describe()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "c0d41fea",
      "metadata": {
        "id": "c0d41fea",
        "outputId": "1f7597a7-0895-456a-8e4c-96bd76438efa"
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 4177 entries, 0 to 4176\n",
            "Data columns (total 9 columns):\n",
            " #   Column          Non-Null Count  Dtype  \n",
            "---  ------          --------------  -----  \n",
            " 0   Sex             4177 non-null   object \n",
            " 1   Length          4177 non-null   float64\n",
            " 2   Diameter        4177 non-null   float64\n",
            " 3   Height          4177 non-null   float64\n",
            " 4   Whole weight    4177 non-null   float64\n",
            " 5   Shucked weight  4177 non-null   float64\n",
            " 6   Viscera weight  4177 non-null   float64\n",
            " 7   Shell weight    4177 non-null   float64\n",
            " 8   Rings           4177 non-null   int64  \n",
            "dtypes: float64(7), int64(1), object(1)\n",
            "memory usage: 293.8+ KB\n"
          ]
        }
      ],
      "source": [
        "df.info()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "4c15561f",
      "metadata": {
        "id": "4c15561f",
        "outputId": "9b035193-8479-406c-f864-ba4a04e74a7f"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Index(['Sex', 'Length', 'Diameter', 'Height', 'Whole weight', 'Shucked weight',\n",
              "       'Viscera weight', 'Shell weight', 'Rings'],\n",
              "      dtype='object')"
            ]
          },
          "execution_count": 7,
          "metadata": {},
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.columns"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "967bc04f",
      "metadata": {
        "id": "967bc04f",
        "outputId": "003f6d6d-b580-4ec5-b2ae-7f40b2942343"
      },
      "outputs": [
        {
          "data": {
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
              "      <th>Sex</th>\n",
              "      <th>Length</th>\n",
              "      <th>Diameter</th>\n",
              "      <th>Height</th>\n",
              "      <th>Whole weight</th>\n",
              "      <th>Shucked weight</th>\n",
              "      <th>Viscera weight</th>\n",
              "      <th>Shell weight</th>\n",
              "      <th>Rings</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>M</td>\n",
              "      <td>0.455</td>\n",
              "      <td>0.365</td>\n",
              "      <td>0.095</td>\n",
              "      <td>0.5140</td>\n",
              "      <td>0.2245</td>\n",
              "      <td>0.1010</td>\n",
              "      <td>0.15</td>\n",
              "      <td>15</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>M</td>\n",
              "      <td>0.350</td>\n",
              "      <td>0.265</td>\n",
              "      <td>0.090</td>\n",
              "      <td>0.2255</td>\n",
              "      <td>0.0995</td>\n",
              "      <td>0.0485</td>\n",
              "      <td>0.07</td>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  Sex  Length  Diameter  Height  Whole weight  Shucked weight  Viscera weight  \\\n",
              "0   M   0.455     0.365   0.095        0.5140          0.2245          0.1010   \n",
              "1   M   0.350     0.265   0.090        0.2255          0.0995          0.0485   \n",
              "\n",
              "   Shell weight  Rings  \n",
              "0          0.15     15  \n",
              "1          0.07      7  "
            ]
          },
          "execution_count": 8,
          "metadata": {},
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.head(2)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "63495ecf",
      "metadata": {
        "id": "63495ecf",
        "outputId": "8e80021a-a765-466e-cef0-41aacd9e6c30"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Length', ylabel='Density'>"
            ]
          },
          "execution_count": 9,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df.Length)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "d10849fa",
      "metadata": {
        "id": "d10849fa",
        "outputId": "e6f2149b-024a-48d8-9c3a-d5f5429e549f"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Diameter', ylabel='Density'>"
            ]
          },
          "execution_count": 10,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df.Diameter)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "31e7a2fe",
      "metadata": {
        "id": "31e7a2fe",
        "outputId": "d23188c7-01ad-4db0-d8a5-60d1ae930057"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Height', ylabel='Density'>"
            ]
          },
          "execution_count": 11,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df.Height)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "887ab0fb",
      "metadata": {
        "id": "887ab0fb",
        "outputId": "4bd1a6b1-aa5a-45a2-a08b-2892cfc26fb8"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Whole weight', ylabel='Density'>"
            ]
          },
          "execution_count": 12,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df['Whole weight'])"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "421c3d9d",
      "metadata": {
        "id": "421c3d9d",
        "outputId": "09226372-1d86-4a13-8e6e-1f9f017f9b3f"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Viscera weight', ylabel='Density'>"
            ]
          },
          "execution_count": 13,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAYIAAAEGCAYAAABo25JHAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjQuMywgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/MnkTPAAAACXBIWXMAAAsTAAALEwEAmpwYAAAtfUlEQVR4nO3deZhcdZno8e9b1Wt635f0loSkE8KWpLOwhIngICAO4wwwgIqikGFER+eOjo4zl3H0Ojr3Oj4joCIKCiqIAoPIBBAXJIEkpAlZCSF70ulOekl39b6/9486YTqdTnelu0+dWt7P85ynTtU5VfXmJDlv/XZRVYwxxsQvn9cBGGOM8ZYlAmOMiXOWCIwxJs5ZIjDGmDhnicAYY+JcgtcBnK38/HytqqryOgxjjIkqb7zxRrOqFox1LOoSQVVVFbW1tV6HYYwxUUVEDp3pmFUNGWNMnLNEYIwxcc61RCAiKSLyuohsFZGdIvKvY5yzSkQCIrLF2e5xKx5jjDFjc7ONoA+4QlU7RSQRWCciz6vqhlHnrVXV61yMwxhjzDhcSwQanMSo03ma6Gw2sZExxkQYV9sIRMQvIluARuAlVd04xmkXO9VHz4vIwjN8zmoRqRWR2qamJjdDNsaYuONqIlDVIVW9CCgDlonIeaNO2QxUquqFwH3AM2f4nAdVtUZVawoKxuwGa4wxZpLC0mtIVduAl4GrR73erqqdzv4aIFFE8sMRkzHGmCA3ew0ViEi2s58KvBd4e9Q5xSIizv4yJ54Wt2IyxhhzOjd7DZUAj4iIn+AN/heq+pyI3AWgqg8ANwB/IyKDQA9ws9pKORHhsY2Hz3js1uUVYYzEGOM2N3sNbQMWjfH6AyP27wfudysGY4wxE7ORxcYYE+csERhjTJyzRGCMMXEu6qahNtNrvEZhY0x8sBKBMcbEOSsRmFMEegY42NJFU0cfw8NKZmoiZTmpzMxOxRnyYYyJMZYIYsRU+/0fauniD7sb2XO8892ZAYX/mSUwLy2Ji+fksWxW7pRjNcZEFksEca6jd4DntjWw/WiAjOQEVlUXcm5pJsWZKfgkWELY19TFpoMneG5bA6/ta+GcwnQumWMzgRgTKywRxLHf7DzGt3+3h/7BYa6cX8jKuQUkJZzabJQ9I4kllUksrshmT2Mnv95az60/2MidK2fxhavnk+C3ZiZjop0lgjg0MDTMvz//Nj9cd4DS7BRuXFJOUWbKuO8REeYVZfDpK+ayr6mTH6w9wFsN7dx/y2Jy0pLCFLkxxg2WCOLM0bYePv3YZjYfbuO2iys5pyD9rH7VJyX4+Oqfn8cFZVn803/t4PrvvMoPbquhujjDxaiNMW6ycn0ceWFHA9d+ey3vHO/k/lsX8ZXrz5t01c6NNeX8/K9X0DMwxA3fe41NB09Mc7TGmHCxRBAHegeG+Kf/2s5dP91MZd4M/vtvL+O6C0qn/LmLK3J45u5LKchI5iMPbeTl3Y3TEK0xJtwsEcS4fU2dXPPttfxs42H++vLZPHnXJVTmpU3b58/MTuUXd13M7Px07ny0ljXbG6bts40x4WFtBDGqs2+Q57c38OaRNirzZvCzO5Zz6TnudPnMT0/m8dUr+MSPN/Gpxzbz4RWVzC/OPO08W8fAmMhkJYIYo6q8caiV//ztO2yrC7CquoAXP3u5a0ngpKzURH50+1JKslJ5/PXDHGrpcvX7jDHTxxJBDGnq6OOH6w7w1OY6CtKT+dQV53DVucWkJPrD8v0ZKYl89JIqslITeWT9QRoCPWH5XmPM1FgiiAF9g0P8dtdx7v39HhoCPXzwopncefnsCccGuCE9OYHbL51Fkt/HI68dpKN3IOwxGGPOjrUReGSycwONft++pk5+taWe5s4+LijL4v3nl5CRkjhtcYYSw2g5M5L46CVVPPDHfTz2+mHuuGw2fp9NWGdMpLJEEKWOtffy4o5j7D7eQc6MRD52SRXziiJnUFdJVip/saiMJ2qP8Ju3jnHNeSVeh2SMOQNLBFFEValr7Wb9vha2HGkjOdHH1QuLuXhOHokROOfPheXZHGjuYt2eZqojKEkZY07lWiIQkRTgFSDZ+Z4nVfVfRp0jwLeBa4Fu4GOqutmtmKJRa1c/r+5rZt2eZtbuaeZoWw9Jfh+XzMnjPdWFzEiO7Fx+7fkl7Gvq5Jdv1DEzO5XkMRqurVupMd5y8y7SB1yhqp0ikgisE5HnVXXDiHOuAeY623Lge85jXOvpH+KZLUf5Ze0R3jzShipkpCRw6Zx8llXlcn5ZVth6Ak1VUoKPG5eU8f1X9vPbXcd5/zSMaDbGTC/XEoGqKtDpPE10Nh112vXAo865G0QkW0RKVDVuh6ceOdHN++9dy/7mLqqLMvjslfNYOS+fC2ZmkeD3ReUawxV5aSytyuW1fS0sqsihNDvV65CMMSO4Wq8gIn7gDeAc4DuqunHUKTOBIyOe1zmvnZIIRGQ1sBqgoiJ2qxH2N3fyo1cPUpSRzKMfX8bKufnTsjxkJCSP9y0sZmdDO89ta+DOlbNs2UtjIoirLYyqOqSqFwFlwDIROW/UKWPdDUaXGlDVB1W1RlVrCgoKXIjUe80dffxsw2FyZySx5jMruXxeQUzdLFOT/Lx3QSEHW7rY1dDhdTjGmBHC0tVEVduAl4GrRx2qA8pHPC8D6sMRUyRRVZ5+8ygi8NFLqsieEZsLvdRU5lKQnswLO48xNHxavjfGeMS1RCAiBSKS7eynAu8F3h512rPAbRK0AgjEY/vA28c6ONjSxXsXFJEbw6t9+X3C+xYW0dzZx9YjbV6HY4xxuNlGUAI84rQT+IBfqOpzInIXgKo+AKwh2HV0L8Huo7e7GE9EGlblhZ3HyE9PYmlVrtfhuG5BSSalWSn8fncjF5Zn24hjYyKAm72GtgGLxnj9gRH7CtztVgzRYM/xTpo6+virpeVxcVMUEa5cUMRPNhxi65E2FlfmeB2SMXEv8oajxpnNh1uZkeRnYenp8/fHqvnFGZRkpfDKniaCvwWMMV6K7GGpMa67f5C3GtpZVpVLgu9/cnIkdPd0k4hw6Zx8ntxcx97GzonfYIxxlZUIPLStLsDQsLIkDqtHLijLIiM5gVf3NXsdijFxzxKBh3bUByjMSKYkK/zrBngtwe9j+ew83jneyZ7jNq7AGC9Z1ZDLzlTN0z84zKGWbi6enRdTA8fOxvJZuby8u5GHXz3A1//iAq/DMSZuWYnAI/ubOxkaVuYWpXsdimfSkhNYVJHD05uP0tLZ53U4xsQtSwQe2XO8k0S/UJWX5nUonrp0Th59g8P8LMYbyI2JZJYIPLKnsZNZ+WkRuaBMOBVmprBybj6Pv37Ypp0wxiPxfRfySFt3P82dfcwttFW7AD60vIKGQC8v7270OhRj4pIlAg8cPtENQFV+fFcLnXTlgiIKMpJ5/HWrHjLGC5YIPHDkRDcJPqE4M/66jY4l0e/jppoyfv92Iw2BHq/DMSbuWCLwwJHWHkqzU+NibqFQ3by0AgWe2HRkwnONMdPLEkGYDQ0r9W09lOfYco0jlefOYOXcAp7YdMQajY0JM0sEYXYs0MvgsFKeO8PrUCLOrcvKaQj08sd3rNHYmHCyRBBmR1qDDcXlOZYIRjvZaBzrk+4ZE2ksEYRZXWs3ackJZM9I9DqUiJPo93HDkjL+sLuJxvZer8MxJm5YIgizhkAvM7NT4nZ+oYncVFPO0LDy5OY6r0MxJm7YpHNhNDSsNHX0cU5h/M4vNJbRVUGz8tN4aO0BslIS+dCKSo+iMiZ+WIkgjFo6+xgcVops/MC4aipzaOnq50BLl9ehGBMXLBGE0TGn3tsGko1vYWkWKYk+ag+2eh2KMXHBEkEYHW/vwydQkJHsdSgRLSnBx4Vl2ew4GiDQM+B1OMbEPNcSgYiUi8gfRGSXiOwUkc+Mcc4qEQmIyBZnu8eteCLB8fZe8tKS437G0VDUVOUyOKw8u+Wo16EYE/PcvCMNAn+vqguAFcDdInLuGOetVdWLnO0rLsbjuWPtvRTF4bKUkzEzO5WSrBSeqLUpJ4xxm2u9hlS1AWhw9jtEZBcwE3jLre+MZP2Dw7R29bOoItvrUKJGTWUOv97WwDdf3E1p9ulTcty6vMKDqIyJPWGpoxCRKmARsHGMwxeLyFYReV5EFp7h/atFpFZEapuamtwM1TWNHb0oUJRhJYJQXVSeQ4JPqD10wutQjIlpricCEUkHngI+q6rtow5vBipV9ULgPuCZsT5DVR9U1RpVrSkoKHA1Xrc0O2vyFlpDcchSk/wsLM1ky5E2BoaGvQ7HmJjlaiIQkUSCSeBnqvr06OOq2q6qnc7+GiBRRPLdjMkrzZ39CJCbluR1KFGlpiqX3oFhdtYHvA7FmJjlZq8hAR4Cdqnqt85wTrFzHiKyzImnxa2YvNTc2UdOWhIJ1mPorMzKTyM3LYlNNqbAGNe4OcXEpcBHgO0issV57UtABYCqPgDcAPyNiAwCPcDNqhqTk9E3d/aRn26lgbPlE6GmMoffvHWcls4+8tKtas2Y6eZmr6F1wLgzq6nq/cD9bsUQKVSV5s5+KvNsjeLJWFSRw0tvHeeNQ61ctbDY63CMiTlWTxEGHX2D9A8Ok2+/ZiclKzWR6uIM3jjcaquXGeMCSwRhcLLHkFUNTV5NZQ4dvYPsOd7hdSjGxBxLBGHQ0tkPQH6alQgmq7o4k/TkBDYdskZjY6abJYIwaO7sI8EnZNmqZJPm9wmLK7LZfaydjl6biM6Y6WSJIAyaO/vJTUvCZ6uSTcmSylyGFTYfbvM6FGNiiiWCMDjR1UeeDSSbsoKMZKryZlB78AQx2svYGE9YInCZqnKiq99GFE+TmspcWrr6OdjS7XUoxsQMSwQu6+ofYmBIybFEMC3Om5lFcoKP2oM2EZ0x08USgctOdAV7DOXOsEQwHZISfFxYns32o4F3r60xZmosEbis1blZWYlg+lw8O4/BYeXx1w97HYoxMcESgctau51EYCWCaVOUmcI5hek8uv6gTU9tzDSwROCyE139pCcnkJRgl3o6XTonj+PtfazZ3uB1KMZEPbs7uexEt/UYcsPcogxm56fx8KsHvQ7FmKhnicBlrV395NiI4mnnE+Fjl1ax9Ugbmw/btBPGTIUlAhcNDg0T6BmwhmKX/OXiMjJSEnh43QGvQzEmqlkicFFDoJdhta6jbklLTuDmpeU8v+MYR9t6vA7HmKjl5gplce/wieDoVysRuOejl1Tx0LoDPPraQf7x2gUAPLbxzN1Kb11eEa7QjIkaViJw0clfqdZ11D1lOTO45rwSHnv9MF19g16HY0xUskTgoqOtPQiQmWoFLzd9/LJZdPQO8uQbdV6HYkxUskTgovq2HjJSEkjw2WV205LKHC4qz+ZHrx6wpSyNmYSQ7lAi8pSIvF9E7I52Fo629ZBt1UJhccfKWRxs6eZ3u457HYoxUSfUG/v3gFuBPSLyDRGZP9EbRKRcRP4gIrtEZKeIfGaMc0RE7hWRvSKyTUQWn2X8Ea2+rYesVBtDEA5XLyxmZnYqD1lXUmPOWkiJQFV/q6ofAhYDB4GXROQ1EbldRM50pxsE/l5VFwArgLtF5NxR51wDzHW21QQTTkwYHlbq23ptMFmYJPh9fPSSSjYeOEG9dSU15qyEXNUjInnAx4A7gDeBbxNMDC+Ndb6qNqjqZme/A9gFzBx12vXAoxq0AcgWkZKz/UNEouauPvqHhsmyqqGw+aulFcxI8vPq3mavQzEmqoTaRvA0sBaYAXxAVf9MVZ9Q1U8D6SG8vwpYBGwcdWgmcGTE8zpOTxaIyGoRqRWR2qamplBC9tzRVqfrqFUNhU1WaiI31ZSzrS5Ae48tcG9MqEItEfxQVc9V1a+ragOAiCQDqGrNeG8UkXTgKeCzqto++vAYbzmt24eqPqiqNapaU1BQEGLI3qpv6wUgy6qGwur2S6sYVmXjgRavQzEmaoSaCP7PGK+tn+hNTvvBU8DPVPXpMU6pA8pHPC8D6kOMKaIdbQuOKs5OtaqhcKrMS2NuUTpvHGq1rqTGhGjcRCAixSKyBEgVkUUistjZVhGsJhrvvQI8BOxS1W+d4bRngduc3kMrgMDJEke0q2/rJSM5gdQkv9ehxJ2lVbm09w7yzvEOr0MxJipMNOT1fQQbiMuAkTfzDuBLE7z3UuAjwHYR2eK89iWgAkBVHwDWANcCe4Fu4PbQQ49sda09lGaneh1GXJpfnEl6cgK1B0+woCTT63CMiXjjJgJVfQR4RET+UlWfOpsPVtV1jN0GMPIcBe4+m8+NFvVtPZRmp3gdRkw70+Ryfp+wuCKHdXub6OgdICPF2mmMGc9EVUMfdnarROR/jd7CEF/Uagj0UGIlAs8srshmWGFbXcDrUIyJeBM1Fqc5j+lAxhibGUPvwBCt3QOUZlmJwCuFmSmUZqWwta7N61CMiXgTVQ1933n81/CEExsaAsGuo8VZqfQPDnscTfy6qDybNTuO0dzRR35GstfhGBOxQh1Q9n9FJFNEEkXkdyLSPKLayIzSEAgOJrMSgbcuKMtGgC1WKjBmXKGOI7jKGQx2HcG+//OAz7sWVZQ79m6JwBKBlzJTE6nKT2PHUWsnMGY8oSaCk90urgUeV9UTLsUTE05WDZVkWWOx1xaWZtLY0UdjR6/XoRgTsUJNBL8WkbeBGuB3IlIA2P+sM2gI9JA9I9EGk0WAhaVZALxVP3p2E2PMSaFOQ/1F4GKgRlUHgC6CM4eaMTS09VKcadVCkSArNZHynFR21Fv1kDFncjaL6S4gOJ5g5HseneZ4YkJDoNdGFUeQhaVZvLDzGK3d/V6HYkxECrXX0E+AbwKXAUudbdxZR+NZQ6DHGoojyPyS4JAXm3vImLGFWiKoAc51poQw47DBZJGnID2ZnBmJ7D5micCYsYTaWLwDKHYzkFhxbMRgMhMZRIR5RRnsa+qkb3DI63CMiTihJoJ84C0ReVFEnj25uRlYtKq3wWQRqbo4g4Eh5fUD1vPZmNFCrRr6sptBxBIbTBaZZuenk+AT/vB2EyvnRscqd8aES6jdR/8IHAQSnf1NwGYX44paNpgsMiUl+JiVn8bLuxu9DsWYiBNqr6E7gSeB7zsvzQSecSmmqGaDySJXdXEG+5u7ONTS5XUoxkSUUNsI7ia44lg7gKruAQrdCiqaHQvYYLJIVV0U7Eb68u4mjyMxJrKEmgj6VPXd0TjOoDLrSjqG+jYbTBap8tKTqcqbwR+sesiYU4TaWPxHEfkSwUXs/xT4JPBr98KKLiOXTDzY0kV6csIZl1E03lpVXcjjrx+md2CIlESrvjMGQi8RfBFoArYDf01w0fl/diuoaDUwNEx3/xBZM2yN3Ei1qrqAvsFhNuxv8ToUYyJGSCUCVR0WkWeAZ1TVKljPoL1nAIAsWyw9Yi2flUdSgo9X3mlmVbU1cxkDEy9eLyLyZRFpBt4GdotIk4jcM9EHi8jDItIoIjvOcHyViAREZIuzTfiZka7NSQSZqZYIIlVqkp/ls3JZu8d+zxhz0kRVQ58l2FtoqarmqWousBy4VET+boL3/hi4eoJz1qrqRc72lVACjmQnSwTZlggi2uVzC9jT2El9W4/XoRgTESZKBLcBt6jqgZMvqOp+4MPOsTNS1VeAuBrPH7ASQVS4fF5wZLGVCowJmigRJKpq8+gXnXaC6bjbXSwiW0XkeRFZOA2f56lAzwCpiX6SEkJtgzdemFeUTlFmMq+8c9o/bWPi0kR3rPFW8pjqKh+bgUpVvRC4j3FGKovIahGpFZHapqbI/RUX6Bkgy0oDEU9EWDm3gHV7mxkatuEwxkyUCC4UkfYxtg7g/Kl8saq2q2qns78GSBSR/DOc+6Cq1qhqTUFB5E4YZokgelw+r4BAzwDb6tq8DsUYz42bCFTVr6qZY2wZqjqlO56IFIuIOPvLnFiiunO3JYLosfKcfESw6iFjCH1A2VkTkceB9UC1iNSJyCdE5C4Rucs55QZgh4hsBe4Fbo7mFdD6B20wWTTJSUvigplZvGINxsac1eL1Z0VVb5ng+P3A/W59f7i9O5jMSgRR4/J5BXz35X1WkjNxz7q3TJM2SwRR5/J5BQwNK6/tteohE99cKxHEm4ANJosKIycDHBpWkhN8PPzqAVq7B7h1eYWHkRnjHSsRTJNAT7A3rQ0mix5+nzCnIJ09jZ1EcfOUMVNmiWCaBHoGSEvyk+i3SxpN5hal09Y9QHPnVIfFGBO97K41TQI9A9ZjKArNLQyuWranscPjSIzxjiWCadLWPUBWapLXYZizlJuWRF5aEu8ct0Rg4pclgmliXRCj1/ziDPY3ddHVN+h1KMZ4whLBNOgdGKJvcNh6DEWpBSWZDA4ra/dYN1ITnywRTIOAjSGIapV5aaQm+nnpreNeh2KMJywRTANLBNHN7xOqizP4/dvHGRwa9jocY8LOEsE0CHQ7icB6DUWtBSWZtHYPUHuo1etQjAk7SwTToK1nAAEybdH6qDWvKJ2URB9rtjd4HYoxYWeJYBoEegbISEnA7xOvQzGTlJzg54r5hazZ3mDVQybuWCKYBoGefmsfiAEfuKCU5s5+Nh6Iq6W2jbFEMB1sDEFseM/8QtKS/Dy3rd7rUIwJK0sEU6SqlghiREqin6sWFvPf2xroHRjyOhxjwsYSwRS1dQ8wMKRkzbDpJWLBjUvKaO8d5IUdx7wOxZiwsUQwRfWBHsDGEMSKFbPzqMidwRObjngdijFhY4lgihraegFbkCZW+HzCTTVlrN/fwsHmLq/DMSYsLBFMUYOVCGLODUvKSfAJj64/5HUoxoSFJYIpqg/04hNIT7FVP2NFcVYK111QwhObDr87fYgxscy1RCAiD4tIo4jsOMNxEZF7RWSviGwTkcVuxeKmhrYeMlMT8YkNJosld6ycTVf/EI+/fnjik42Jcm6WCH4MXD3O8WuAuc62Gviei7G4pj7Qa+0DMei8mVlcMiePh9cdsK6kJua5lghU9RVgvCGa1wOPatAGIFtEStyKxy0NgR5rH4hRf3vlXBo7+nh0/UGvQzHGVV62EcwERvbRq3NeixpDw8qxQK8tURmjVszO4/J5BXz35X2091pbgYldXiaCsSrVdcwTRVaLSK2I1DY1NbkcVugaO3oZGFJy0qxEEKs+f1U1bd0DfOf3e70OxRjXeJkI6oDyEc/LgDEneVHVB1W1RlVrCgoKwhJcKOpag11Hc2xUccw6vyyLm2rKeGjdAVvg3sQsLxPBs8BtTu+hFUBAVaNqMvi61m7AEkGs++I1C0hPSeCf/2sHw8NjFlqNiWpudh99HFgPVItInYh8QkTuEpG7nFPWAPuBvcAPgE+6FYtb6k4ESwTZtjJZTMtNS+JL1y7g9YMn+NFrB70Ox5hp59ooKFW9ZYLjCtzt1veHQ11rD4UZyST6bVxeLHhs45nHDKgq84sz+PqaXbT3DFCUmQLArcsrwhWeMa6xO9gU1LV1U5aT6nUYJgxEhA8umklyop/HNh6mz8YWmBhiiWAK6lp7KMuZ4XUYJkwyUhK5ZWk5LV19PLW5jmCh1pjoZ4lgkoaGlfq2HisRxJnZBem8b2ExO+rbWbe32etwjJkWlggm6Xh7cAyBlQjiz2Xn5LOwNJMXdx5j/b4Wr8MxZsosEUzSyTEEViKIPyLCDYvLyEtL5u7HNnO0rcfrkIyZEksEk3RyDIElgviUnOjnwysqGRgcZvWjtfT0W+OxiV6WCCbpyIkeRKA02xJBvCrISObbt1zEWw3t/OPT26zx2EQtSwSTdKili9KsVFIS/V6HYjx0xfwiPndVNc9sqeeHaw94HY4xk2LLak3SwZYuKvOsoTjePbbxMNmpiZxXmsm/rdlFfaCHuYUZgA02M9HDSgSTdKilm8q8NK/DMBFARPjLJWUUZibz89ePcKKr3+uQjDkrlggmob13gJaufqqsRGAcyQl+Pry8EoCfbjhE36A1HpvoYYlgEg63BHsMWYnAjJSXnszNS8s53t7L05uPWuOxiRqWCCbhYEsXAFX5ViIwp5pblMGfnlvE9qMBfjrOJHbGRBJLBJNwyCkRVORaIjCnu3xeAfOK0vnqr99ix9GA1+EYMyFLBJNwsLmLosxkZiRZpytzOp8INy4pJzctibsf22zrHZuIZ4lgEqzHkJlIWnIC9926iLrWHv7xqe3WXmAimv2knYSDLV2sqo6ctZNNZNpzvJP3Lijiv7c34Pu5cPHsvHeP2RgDE0msRHCW2nsHaOzooyrfSgRmYivn5lNdlMGa7Q0cbbXJ6UxkskRwlvY2dgIwzxk9asx4gu0FZaQl+fn5JlvZzEQmSwRnae/xYCKYW5TucSQmWsxITuCvllZwoqufX22tt/YCE3EsEZylPY0dJCf4bEEac1Zm5adxxYJCthxp483DbV6HY8wpLBGcpT2NncwpSMfvE69DMVHmPdWFzMpP41dbj7KvqdPrcIx5l6uJQESuFpHdIrJXRL44xvFVIhIQkS3Odo+b8UyHPcc7rVrITIpPhJtqykn0+/jUY2/Sa+0FJkK4lghExA98B7gGOBe4RUTOHePUtap6kbN9xa14pkNX3yBH23qYW2iJwExOVmoiNywpY1dDO19fs8vrcIwB3C0RLAP2qup+Ve0Hfg5c7+L3ue5kcf4c6zFkpmB+cSafuGwWj6w/xIs7j3kdjjGuJoKZwJERz+uc10a7WES2isjzIrJwrA8SkdUiUisitU1NTW7EGpI91mPITJN/uLqa82dm8Q9PbuNom40vMN5yMxGM1Zo6ut/cZqBSVS8E7gOeGeuDVPVBVa1R1ZqCAu9G9L5zvIMkv88mmzNTlpzg575bFjE4NMxnHn+TwaFhr0MycczNRFAHlI94XgbUjzxBVdtVtdPZXwMkiki+izFNyfajAaqLM0j0W2crMzWPbTzMa/taeP8FpdQeauXOR2t5zKatNh5x8462CZgrIrNEJAm4GXh25AkiUiwi4uwvc+JpcTGmSVNVdhwNcN7MLK9DMTHkovJsllTk8PLuJutSajzjWiJQ1UHgU8CLwC7gF6q6U0TuEpG7nNNuAHaIyFbgXuBmjdBhl0dO9NDeO8j5lgjMNPvAhaXkpSfzi01HaO7s8zocE4dcreNQ1TWqOk9V56jq15zXHlDVB5z9+1V1oapeqKorVPU1N+OZiu3OAiOWCMx0S0rwccuycnoGhvjcL7cyPByRv4VMDLPK7hBtPxog0S/MK7YeQ2b6lWSlcu35Jby8u4kfrtvvdTgmzth6BCHa4TQUJyf4vQ7FxKjls3LpGxzi31/YzdzCDN4zv9DrkEycsBJBCFSVHfUBziu1aiHjHhHhP266iPnFGXzyZ5vZeqTN65BMnLBEEIL9zV20dQ9wYXm216GYGJeenMCPbl9KXnoSH//xJg61dHkdkokDlghCsGF/sEfrihFLDRrjlsKMFB75+DKGVLnt4dept5HHxmWWCEKwYf8JijKTqcqzEcUmPOYUpPPwx5ZyorOfGx9Yz4FmKxkY91gimICqsn5fCytm5+GMfTMmLBZX5PDYnSvo7h/kg999lfX7InKspYkB1mtoAvuaumju7LNqIRMWY00z8fFLZ/Ho+kN8+KGNfPbKuXzyPefYwkhmWlmJYALWPmC8lpeezN+smsP7zy/hP156hw9+91XrUWSmlSWCCby8u4nSrBRrHzCeSkn0c+8ti7jvlkU0BHq5/juvsvrRWrbVtXkdmokBVjU0jo7eAV7Z08SHlldY+4CJCB+4sJQ/qS7gobUH+NGrB/jNW8dZOTefytw0qoszTqsyunV5hUeRmmhiiWAcv3+7kf7BYa49v8TrUIx5V2ZKIn/3p/O4Y+UsfrrhMD969QBr9zSTkZzA4socaipzyEtP9jpME0Wsamgcz28/RmFGMksqcrwOxZjTZKQk8jer5vDaF6/gIysqmZmTyivvNPEfL73DD9fuZ8uRNnoHhrwO00QBKxGcQWffIC+/08hNNeX4rIeGiQDjLVyzoCSTBSWZBHoGePNwK7WHWvlF7RFe3HmMP7+olBuWlHPezEyr4jRjskRwBk9sOkLvwDB/sbjM61CMCVlWaiKrqgu5fF4BB5q7aOzo4/FNR3hk/SGqizK4YUkZ1y8qpTAjZdzEYm0L8cUSwRgGh4Z5eN0BllblcJHNL2SikE+EOQXp/O/rziXQPcBz2+t58o06vrZmF9944W1WzSugMDOF6qIMkhKshjjeWSIYwws7j3G0rYd7PnCu16EYM2VZMxL50PJKPrS8kr2NnTy1uY6nN9dx/O1GEv1CdXEm58/MsqQQxywRjNI7MMQ3X9zN7Pw03rugyOtwjJlW5xSm84Wr5/O5q6r5tzW72HE0wI76dnY4Cy9VF2Vw3swsrr+olLRkuz3EC/ubHuXe3+3hYEs3P7tjuQ3jNzHL7wtWHc0pSOcDF5ZysLmL7UcD7KxvZ0d9O89sOcqqeYVce0EJV84vtKQQ4+xvd4RX3mni+6/s58YlZVx6Tr7X4RgzZeM1CJ/kE2F2QTqzTyaFli76B4d5fscxXth5jOQEH6uqC7jmvBJWzs23MQoxyBKB47W9zaz+SS3VRRn883XWNmDik0+E2fnp3Lq8gn/5wELeONTKmu0NPL+jgRd3HgeCXVUvmZPHBWVZnFuSyaz8NBL81rYQzVxNBCJyNfBtwA/8UFW/Meq4OMevBbqBj6nqZjdjGq21q58H/riPH6zdz+yCdB79xDKyUhPDGYIxEcnvE5bNymXZrFzuue5ctta18dq+Fl7d28xPNhyif3AYgKQEH2XZqZRmpzLTeSzJSiEvPYm89GTy05PIT08mJdHW+45UriUCEfED3wH+FKgDNonIs6r61ojTrgHmOtty4HvO47RTVU509XOsvZfj7b0caunm9QMn+P3bjfQNDnPLsgr+6f0LSLe6UGPOWKWUMyOJ6y4o5ZrzSmjq6KMh0MOxQC+tPQMcauli65E2OvoGx3xvWpKf/Ixk8tJOTRAZKQmkJiUwI9HPjCQ/KUl+/CL4fYLPefT7eHf/f14bsS+Cz8eI/RGPI4875080sE5VGVYYVmVYFXX2h4ZP3QZPeRx+9/nJ1wASnDgT/PLu95+2iZDg8+HzceqjEJZBgG7e9ZYBe1V1P4CI/By4HhiZCK4HHlVVBTaISLaIlKhqw3QH86st9Xz2iS2nvFaalcKNNWV8eEUl84szp/srjYlZfp9QnJVCcVbKaccGh4bp6B2ks2+Qrr7g48n9jr5BAj0D1Lf1vvuaehC/CO8mCgBG3PSHvQhoHCcThd8n3LlyFv/rqupp/w43E8FM4MiI53Wc/mt/rHNmAqckAhFZDax2nnaKyO7pCPAQsB742tQ/Kh9onvrHxBS7Jqey63E6uyanG/ea/L2zTVLlmQ64mQjGKs+MzrWhnIOqPgg8OB1BuUFEalW1xus4Ioldk1PZ9TidXZPTeXVN3GzqrwPKRzwvA+oncY4xxhgXuZkINgFzRWSWiCQBNwPPjjrnWeA2CVoBBNxoHzDGGHNmrlUNqeqgiHwKeJFg99GHVXWniNzlHH8AWEOw6+hegt1Hb3crHpdFbLWVh+yanMqux+nsmpzOk2siwQ47xhhj4pUNBzTGmDhnicAYY+KcJYIQicjVIrJbRPaKyBfHOC4icq9zfJuILPYiznAK4Zp8yLkW20TkNRG50Is4w2miazLivKUiMiQiN4QzPi+Eck1EZJWIbBGRnSLyx3DHGG4h/N/JEpFfi8hW55q4236qqrZNsBFs7N4HzAaSgK3AuaPOuRZ4nuDYiBXARq/jjoBrcgmQ4+xfY9fklPN+T7CzxA1ex+31NQGyCc44UOE8L/Q67gi4Jl8C/t3ZLwBOAEluxWQlgtC8O12GqvYDJ6fLGOnd6TJUdQOQLSIl4Q40jCa8Jqr6mqq2Ok83EBwnEstC+XcC8GngKaAxnMF5JJRrcivwtKoeBlDVWL8uoVwTBTKciTnTCSaCsSdxmgaWCEJzpqkwzvacWHK2f95PECwxxbIJr4mIzAQ+CDwQxri8FMq/k3lAjoi8LCJviMhtYYvOG6Fck/uBBQQH2G4HPqOqw24FZFNthmbapsuIISH/eUXkPQQTwWWuRuS9UK7JfwJfUNWhcMwqGQFCuSYJwBLgSiAVWC8iG1T1HbeD80go1+R9wBbgCmAO8JKIrFXVdjcCskQQGpsu43Qh/XlF5ALgh8A1qtoSpti8Eso1qQF+7iSBfOBaERlU1WfCEmH4hfp/p1lVu4AuEXkFuBCI1UQQyjW5HfiGBhsJ9orIAWA+8LobAVnVUGhsuozTTXhNRKQCeBr4SAz/uhtpwmuiqrNUtUpVq4AngU/GcBKA0P7v/ApYKSIJIjKD4CzFu8IcZziFck0OEywhISJFQDWw362ArEQQAo2v6TJCEuI1uQfIA77r/AIe1BiebTLEaxJXQrkmqrpLRF4AtgHDBFcz3OFd1O4K8d/JV4Efi8h2glVJX1BV16bstikmjDEmzlnVkDHGxDlLBMYYE+csERhjTJyzRGCMMXHOEoExxsQ5SwQmajlTErxv1GufFZHvisifjTf7Z6QTkddCOOegiOSP8foqEbnEnchMLLJEYKLZ4wQH44x0M/C4qj6rqt9w40tFxPXxN6o6lRv5KoIzvxoTEksEJpo9CVwnIskAIlIFlALrRORjInK/8/qNIrLDmdv9Fec1v4h8U0S2O+slfNp5fYmI/NGZ/OzFkzPIOqWPf3Pmyv+MiHxARDaKyJsi8ltn9OcpRGSNM8UGznn3OPtfFZE7nP3Pi8gmJ4Z/HfHeTufR55RwdorIc85njlzD4NMistn5c8x3rsFdwN9JcH7/ldN4vU2MspHFJmqpaouIvA5cTXCagpuBJ1RVR03odg/wPlU9KiLZzmurgVnAImekZ66IJAL3AderapOI/BXwNeDjznuyVfVPAEQkB1jhfNcdwD8Afz8qxFcITp1wkOAUwpc6r18G/FRErgLmEpyWWIBnReRyVX1lxGf8BVAFnA8UEpx64eERx5tVdbGIfBL4nKreISIPAJ2q+s1Qr6WJb1YiMNFuZPXQzc7z0V4lOFz/ToJD+gHeCzygqoMAqnqC4Hwu5xGc6XEL8M+cuobCEyP2y4AXnSkAPg8sHON71wKXE7zx/zeQ7sylU6Wqu4GrnO1NYDPBScXmjvqMy4Bfquqwqh4D/jDq+NPO4xsEE4YxZ81KBCbaPQN8S4JLg6aq6ubRJ6jqXSKyHHg/sEVELiL4C3ysqcR3qurFZ/iurhH79wHfUtVnRWQV8OUxzt9EcLbR/cBLBGcbvZPgTfvk931dVb8/zp9vormq+5zHIez/s5kkKxGYqKaqncDLBKtLxioNICJzVHWjqt4DNBOcAvg3wF0nG35FJBfYDRSIyMXOa4kiMtYvfYAs4Kiz/9EzxNZPcAGSmwiu0LYW+JzzCMFJxz4uIunO980UkcJRH7MO+EunraCIYEPwRDqAjBDOMwawRGBiw+ME56//+RmO/z+nMXUHwXr7rQTXSDgMbBORrcCtzo37BuDfnde2cObeN18GfikiawkmlzNZCxxX1W5nv8x5RFV/AzxGcCGW7QQbv0ffwJ8iOH/9DuD7wEYgMM73Afwa+KA1FptQ2eyjxkQ4EUlX1U4RySO4MMmlTnuBMdPC6hSNiXzPOb2dkoCvWhIw081KBMYYE+esjcAYY+KcJQJjjIlzlgiMMSbOWSIwxpg4Z4nAGGPi3P8HTzFY4/Q1JGEAAAAASUVORK5CYII=\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df['Viscera weight'])"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "28adb75d",
      "metadata": {
        "id": "28adb75d",
        "outputId": "1c844077-1987-432a-f5b2-d654c7c299e0"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Shell weight', ylabel='Density'>"
            ]
          },
          "execution_count": 14,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.distplot(df['Shell weight'])"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "0acd806e",
      "metadata": {
        "id": "0acd806e",
        "outputId": "c238ff75-1aa8-431c-ba43-a6616778915b"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:>"
            ]
          },
          "execution_count": 15,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "df.Rings.value_counts().plot(kind='barh')"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "b5decab8",
      "metadata": {
        "id": "b5decab8",
        "outputId": "4fae1a52-7af0-4029-a662-970a8f6b6628"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:>"
            ]
          },
          "execution_count": 16,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "df.Sex.value_counts().plot(kind='barh')"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "a6280cdd",
      "metadata": {
        "id": "a6280cdd",
        "outputId": "9a485268-064f-45a8-861f-43bc7e688b6a"
      },
      "outputs": [
        {
          "data": {
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
              "      <th>Sex</th>\n",
              "      <th>Length</th>\n",
              "      <th>Diameter</th>\n",
              "      <th>Height</th>\n",
              "      <th>Whole weight</th>\n",
              "      <th>Shucked weight</th>\n",
              "      <th>Viscera weight</th>\n",
              "      <th>Shell weight</th>\n",
              "      <th>Rings</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>M</td>\n",
              "      <td>0.455</td>\n",
              "      <td>0.365</td>\n",
              "      <td>0.095</td>\n",
              "      <td>0.5140</td>\n",
              "      <td>0.2245</td>\n",
              "      <td>0.1010</td>\n",
              "      <td>0.15</td>\n",
              "      <td>15</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>M</td>\n",
              "      <td>0.350</td>\n",
              "      <td>0.265</td>\n",
              "      <td>0.090</td>\n",
              "      <td>0.2255</td>\n",
              "      <td>0.0995</td>\n",
              "      <td>0.0485</td>\n",
              "      <td>0.07</td>\n",
              "      <td>7</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  Sex  Length  Diameter  Height  Whole weight  Shucked weight  Viscera weight  \\\n",
              "0   M   0.455     0.365   0.095        0.5140          0.2245          0.1010   \n",
              "1   M   0.350     0.265   0.090        0.2255          0.0995          0.0485   \n",
              "\n",
              "   Shell weight  Rings  \n",
              "0          0.15     15  \n",
              "1          0.07      7  "
            ]
          },
          "execution_count": 17,
          "metadata": {},
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.head(2)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "id": "78422974",
      "metadata": {
        "id": "78422974",
        "outputId": "e435255d-6897-43cc-d8b6-3a2fc3b5d7d8"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<AxesSubplot:xlabel='Sex', ylabel='Rings'>"
            ]
          },
          "execution_count": 18,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "needs_background": "light"
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.barplot(x='Sex',y='Rings',data=df)"
      ]
    },
    {
      "cell_type": "code",
  
      "execution_count": null,
      "id": "2fc0e909",
      "metadata": {
        "id": "2fc0e909",
        "outputId": "dd496760-4d61-4e89-8ab5-9f140df625b8"
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "<seaborn.axisgrid.PairGrid at 0x7f791aa66ee0>"
            ]
          },
          "execution_count": 19,
          "metadata": {},
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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
