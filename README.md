{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import spacy\n",
    "nlp=spacy.load(\"en_core_web_lg\")\n",
    "from spacy.lang.en.stop_words import STOP_WORDS\n",
    "list1=[\"Party\",\"Contracting\",\"Paragraph\",\"i\",\"ii\",\"iii\",\"Article\",\"Parties\"]\n",
    "for item in list1:\n",
    "    nlp.Defaults.stop_words.add(item)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "import os\n",
    "import re\n",
    "FET_list=os.listdir(\"BIT text/FET\")\n",
    "FET_list=[name for name in FET_list if name !=\"United States.txt\"]\n",
    "pattern=\"(\\w*\\s?\\w*)\\.txt\"\n",
    "def BITprocessor(text,country):\n",
    "    processing=nlp(text)\n",
    "    extraction=[token.lemma_ for token in processing if token.text not in STOP_WORDS and token.is_punct == False and token.is_stop == False and token.text!=\"\\n\" and token.is_digit==False and len(token.text)>1 and token.is_alpha==True]\n",
    "    extraction2=' '.join(extraction)\n",
    "    document=nlp.make_doc(extraction2)\n",
    "    name=re.findall(pattern,country)\n",
    "    return document, name[0]\n",
    "textUS=open(\"BIT text/FET/United States.txt\",encoding=\"utf8\",mode=\"r\").read() \n",
    "documentUS, name_US=BITprocessor(textUS,country=\"United States.txt\")   "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "spacy_similarity_score_US=[]\n",
    "country_name=[]\n",
    "FET_processed=[]\n",
    "for country_n in FET_list:\n",
    "    text=open(\"BIT text/FET/{}\".format(country_n),encoding=\"utf8\",mode=\"r\").read()\n",
    "    document, name = BITprocessor(text,country=\"BIT text/FET/{}\".format(country_n))\n",
    "    score = document.similarity(documentUS)\n",
    "    FET_processed.append(document.text)\n",
    "    country_name.append(name)\n",
    "    spacy_similarity_score_US.append(score)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['shall accord investment investor fair equitable treatment constant protection security shall unduly discriminatory impair management operation maintenance use enjoyment sale liquidation investment investor shall unduly discriminatory impair management operation maintenance use enjoyment sale liquidation investment investor shall accord investor investment return treatment favourable accord investor investment investor country investment return respect management operation maintenance use enjoyment sale liquidation dispute settlement investment return whichever favourable investor provision Agreement shall construe prevent take action pursuance obligation United Nations Charter maintenance international peace security prevent fulfil obligation member economic integration agreement free trade area customs union common market economic community monetary union European Union oblige extend investor investment return present future benefit treatment preference privilege virtue membership agreement multilateral agreement investment oblige extend investor investment return present future benefit treatment preference privilege result obligation international agreement international arrangement domestic legislation taxation', 'investment investor territory state return relate thereto shall accord treatment favourable Host accord investment return investor investor State whichever favourable investor investor shall accord regard management maintenance use enjoyment disposal investment treatment favourable accord investor investor State whichever favourable investor present shall apply respect kind treatment offer Articles Agreement shall apply respect Investor right submit dispute arise Agreement dispute settlement procedure obligation refer paragraph shall apply treatment accord treaty bilateral multilateral force sign prior date entry force Agreement notwithstanding paragraphs reserve right adopt maintain measure accord differential treatment socially economically disadvantaged rural population marginalize community', 'shall accord cover investment treatment accordance customary international law minimum standard treatment alien include fair equitable treatment protection security concept fair equitable treatment protection security paragraph require treatment addition require customary international law minimum standard treatment alien determination breach provision Agreement separate international agreement establish breach', 'investment investor shall time accord fair equitable treatment shall enjoy protection security territory', 'shall extend fair equitable treatment accordance principle International Law investment investor territory maritime area shall ensure exercise right recognize shall hinder law practice particular exclusively shall consider de jure de facto impediment fair equitable treatment restriction purchase transport raw material auxiliary material energy fuel mean production operation type hindrance sale transport product country abroad measure similar effect framework internal legislation shall favorably examine request entry authorization reside work travel national relation investment territory maritime area', 'State shall territory case accord investment investor State fair equitable treatment protection Treaty State shall territory impair arbitrary discriminatory measure activity investor State regard investment particular management maintenance use enjoyment disposal investment provision shall prejudice return investment return reinvested return shall enjoy protection original investment', 'investment national company shall accord fair equitable treatment protection security territory understand concept fair equitable treatment protection security mean treatment meet standard require customary international law require treatment addition standard shall way impair unreasonable discriminatory measure management maintenance use enjoyment disposal investment territory national company', 'shall subject investment investor measure constitute violation customary international law denial justice judicial administrative proceeding fundamental breach process target discrimination manifestly unjustified ground gender race religious belief iv manifestly abusive treatment coercion duress harassment shall accord territory investment investor respect investment protection security great certainty protection security refer obligation relate physical security investor investment investor obligation whatsoever determination breach provision Treaty separate international agreement establish breach consider allege breach article Tribunal shall account investor appropriate locally establish enterprise pursue action remedy domestic court tribunal prior initiate claim Treaty', 'investment investor territory shall time accord fair equitable treatment protection security accordance paragraph breach obligation fair equitable treatment reference paragraph measure series measure constitute denial justice criminal civil administrative proceeding fundamental breach process include fundamental breach transparency obstacle effective access justice judicial administrative proceeding manifest arbitrariness target discrimination manifestly wrongful ground gender race religious belief harassment coercion abuse power similar bad faith conduct apply fair equitable treatment obligation Tribunal account specific representation investor induce cover investment create legitimate expectation investor rely decide maintain covered investment subsequently frustrate great certainty protection security refer obligation relate physical security investor cover investment great certainty breach provision Agreement international agreement constitute breach', 'shall accord investment investor treatment accordance customary international law include fair equitable treatment protection security great certainty concept fair equitable treatment protection security require treatment addition require customary international law minimum standard treatment alien determination breach provision Agreement separate international agreement establish breach', 'shall ensure fair equitable treatment investment investor addition shall accord investment physical security protection breach aforementioned obligation fair equitable treatment measure series measure constitute denial justice criminal civil administrative proceeding fundamental breach process include fundamental breach transparency judicial administrative proceeding manifest arbitrariness direct target indirect discrimination wrongful ground gender race nationality sexual orientation religious belief abusive treatment investor harassment coercion abuse power corrupt practice similar bad faith conduct breach element fair equitable treatment obligation adopt accordance paragraph shall request review content obligation provide fair equitable treatment complement list joint interpretative declaration meaning paragraph sub Vienna Convention Law Treaties apply paragraph Tribunal account specific representation investor induce investment create legitimate expectation investor rely decide maintain investment subsequently frustrate enter write commitment investor specific investment shall entity exercise governmental authority breach say commitment exercise governmental authority way cause loss damage investor investment great certainty breach provision Agreement international agreement constitute breach addition fact measure breach domestic law establish breach', 'State shall ensure administrative legislative judicial process operate manner arbitrary deny administrative procedural justice process investor State investment take consideration level development State investor investment require circumstance shall notify timely manner administrative judicial proceeding directly affect investment exceptional circumstance notice contrary domestic law administrative decision making process shall include right administrative review appeal decision commensurate level development available resource disposal State Investor Investment shall access government hold information timely fashion accordance domestic law subject limitation access information applicable domestic law state progressively strive improve transparency efficiency independence accountability legislative regulatory administrative judicial process accordance respective domestic law regulation']"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      "['Austria', 'Azerbaijan', 'Canada', 'Czech Republic', 'France', 'Germany', 'Ghana', 'India', 'Luxembourg', 'Mexico', 'Netherlands', 'African Community']\n"
     ]
    }
   ],
   "source": [
    "print(FET_processed)\n",
    "print(country_name)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.feature_extraction.text import TfidfVectorizer\n",
    "from sklearn.cluster import KMeans\n",
    "import pandas as pd\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "  (0, 281)\t0.07299634179927383\n",
      "  (0, 161)\t0.06269010627330317\n",
      "  (0, 89)\t0.049705785092629\n",
      "  (0, 26)\t0.07299634179927383\n",
      "  (0, 255)\t0.07299634179927383\n",
      "  (0, 189)\t0.06269010627330317\n",
      "  (0, 186)\t0.07299634179927383\n",
      "  (0, 304)\t0.07299634179927383\n",
      "  (0, 218)\t0.14599268359854767\n",
      "  (0, 212)\t0.14599268359854767\n",
      "  (0, 35)\t0.14599268359854767\n",
      "  (0, 128)\t0.14599268359854767\n",
      "  (0, 214)\t0.12538021254660633\n",
      "  (0, 113)\t0.12538021254660633\n",
      "  (0, 197)\t0.14599268359854767\n",
      "  (0, 107)\t0.07299634179927383\n",
      "  (0, 188)\t0.07299634179927383\n",
      "  (0, 49)\t0.06269010627330317\n",
      "  (0, 179)\t0.07299634179927383\n",
      "  (0, 48)\t0.07299634179927383\n",
      "  (0, 297)\t0.2189890253978215\n",
      "  (0, 70)\t0.07299634179927383\n",
      "  (0, 24)\t0.06269010627330317\n",
      "  (0, 286)\t0.07299634179927383\n",
      "  (0, 123)\t0.07299634179927383\n",
      "  :\t:\n",
      "  (11, 200)\t0.08708074260208687\n",
      "  (11, 162)\t0.17416148520417374\n",
      "  (11, 257)\t0.0747859532892002\n",
      "  (11, 3)\t0.1495719065784004\n",
      "  (11, 287)\t0.06606266144691991\n",
      "  (11, 222)\t0.23718540248979608\n",
      "  (11, 221)\t0.05929635062244902\n",
      "  (11, 11)\t0.3557781037346941\n",
      "  (11, 156)\t0.17788905186734705\n",
      "  (11, 158)\t0.05929635062244902\n",
      "  (11, 276)\t0.0747859532892002\n",
      "  (11, 23)\t0.0747859532892002\n",
      "  (11, 100)\t0.06606266144691991\n",
      "  (11, 248)\t0.05929635062244902\n",
      "  (11, 142)\t0.05376787213403322\n",
      "  (11, 160)\t0.1801783211670117\n",
      "  (11, 5)\t0.09818722034817935\n",
      "  (11, 258)\t0.06606266144691991\n",
      "  (11, 87)\t0.05929635062244902\n",
      "  (11, 273)\t0.3303133072345995\n",
      "  (11, 89)\t0.23718540248979608\n",
      "  (11, 279)\t0.0747859532892002\n",
      "  (11, 153)\t0.10616462773795347\n",
      "  (11, 152)\t0.12129072580624468\n",
      "  (11, 267)\t0.12129072580624468\n"
     ]
    }
   ],
   "source": [
    "idf=TfidfVectorizer()\n",
    "freq_matrix=idf.fit_transform(FET_processed)\n",
    "print(freq_matrix)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([0, 0, 2, 0, 0, 0, 2, 1, 1, 2, 1, 0])"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "kmeans=KMeans(n_clusters=3)\n",
    "kmeans.fit(freq_matrix)\n",
    "kmeans.predict(freq_matrix)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['Austria', 'Azerbaijan', 'Canada', 'Czech Republic', 'France', 'Germany', 'Ghana', 'India', 'Luxembourg', 'Mexico', 'Netherlands', 'African Community']\n"
     ]
    }
   ],
   "source": [
    "print(country_name)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[[0.         0.         0.         ... 0.         0.         0.        ]\n",
      " [0.         0.         0.00034196 ... 0.         0.         0.        ]\n",
      " [0.         0.07726499 0.07423933 ... 0.         0.03663892 0.07726499]\n",
      " [0.11165729 0.         0.         ... 0.11165729 0.00146389 0.        ]\n",
      " [0.         0.         0.00107482 ... 0.         0.         0.        ]]\n"
     ]
    }
   ],
   "source": [
    "from sklearn.decomposition import NMF\n",
    "nmf=NMF(n_components=5)\n",
    "NMF_matrix=nmf.fit_transform(freq_matrix)\n",
    "print(nmf.components_)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['abroad', 'abuse', 'abusive', 'access', 'accord', 'accordance', 'account', 'accountability', 'action', 'activity', 'addition', 'administrative', 'adopt', 'affect', 'aforementioned', 'agreement', 'alien', 'allege', 'appeal', 'applicable', 'apply', 'appropriate', 'arbitrariness', 'arbitrary', 'area', 'arise', 'arrangement', 'article', 'articles', 'authority', 'authorization', 'auxiliary', 'available', 'bad', 'belief', 'benefit', 'bilateral', 'breach', 'case', 'cause', 'certainty', 'charter', 'circumstance', 'civil', 'claim', 'coercion', 'commensurate', 'commitment', 'common', 'community', 'company', 'complement', 'concept', 'conduct', 'consider', 'consideration', 'constant', 'constitute', 'construe', 'content', 'contrary', 'convention', 'corrupt', 'country', 'court', 'cover', 'covered', 'create', 'criminal', 'customary', 'customs', 'damage', 'date', 'de', 'decide', 'decision', 'declaration', 'denial', 'deny', 'determination', 'development', 'differential', 'direct', 'directly', 'disadvantaged', 'discrimination', 'discriminatory', 'disposal', 'dispute', 'domestic', 'duress', 'economic', 'economically', 'effect', 'effective', 'efficiency', 'element', 'energy', 'enjoy', 'enjoyment', 'ensure', 'enter', 'enterprise', 'entity', 'entry', 'equitable', 'establish', 'european', 'examine', 'exceptional', 'exclusively', 'exercise', 'expectation', 'extend', 'fact', 'facto', 'fair', 'faith', 'fashion', 'favorably', 'favourable', 'force', 'framework', 'free', 'frustrate', 'fuel', 'fulfil', 'fundamental', 'future', 'gender', 'government', 'governmental', 'great', 'ground', 'harassment', 'hinder', 'hindrance', 'hold', 'host', 'impair', 'impediment', 'improve', 'include', 'independence', 'indirect', 'induce', 'information', 'initiate', 'integration', 'internal', 'international', 'interpretative', 'investment', 'investor', 'iv', 'joint', 'judicial', 'jure', 'justice', 'kind', 'law', 'legislation', 'legislative', 'legitimate', 'level', 'limitation', 'liquidation', 'list', 'locally', 'loss', 'maintain', 'maintenance', 'making', 'management', 'manifest', 'manifestly', 'manner', 'marginalize', 'maritime', 'market', 'material', 'mean', 'meaning', 'measure', 'meet', 'member', 'membership', 'minimum', 'monetary', 'multilateral', 'national', 'nationality', 'nations', 'notice', 'notify', 'notwithstanding', 'obligation', 'oblige', 'obstacle', 'offer', 'operate', 'operation', 'orientation', 'original', 'paragraph', 'paragraphs', 'particular', 'peace', 'physical', 'population', 'power', 'practice', 'preference', 'prejudice', 'present', 'prevent', 'principle', 'prior', 'privilege', 'procedural', 'procedure', 'proceeding', 'process', 'product', 'production', 'progressively', 'protection', 'provide', 'provision', 'purchase', 'pursuance', 'pursue', 'race', 'raw', 'recognize', 'refer', 'reference', 'regard', 'regulation', 'regulatory', 'reinvested', 'relate', 'relation', 'religious', 'rely', 'remedy', 'representation', 'request', 'require', 'reserve', 'reside', 'resource', 'respect', 'respective', 'restriction', 'result', 'return', 'review', 'right', 'rural', 'sale', 'say', 'security', 'separate', 'series', 'settlement', 'sexual', 'shall', 'sign', 'similar', 'socially', 'specific', 'standard', 'state', 'strive', 'sub', 'subject', 'submit', 'subsequently', 'take', 'target', 'taxation', 'territory', 'thereto', 'time', 'timely', 'trade', 'transparency', 'transport', 'travel', 'treaties', 'treatment', 'treaty', 'tribunal', 'type', 'understand', 'unduly', 'union', 'united', 'unjustified', 'unreasonable', 'use', 'vienna', 'violation', 'virtue', 'way', 'whatsoever', 'whichever', 'work', 'write', 'wrongful']\n"
     ]
    }
   ],
   "source": [
    "print(idf.get_feature_names())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAX0AAAD4CAYAAAAAczaOAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjEsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy8QZhcZAAAgAElEQVR4nO29efwkVXnv/znd/V1mX5hhHWAGHFCuC8qARr2Ka/AaQSN4MYlXfhIxMdzEGBPxJuEXyU1iXECjCBKjqNGwiAvqIIrCsMPMsA0zzM4s31m/s3znu/a3l3ruH+ecqlPV1dVV1VXd1d3P+/Wa6eVbXfVU1amnnvqc5zxHEBEYhmGY3iDXbgMYhmGY1sFOn2EYpodgp88wDNNDsNNnGIbpIdjpMwzD9BCFdm140aJFtHTp0nZtnmEYpiNZu3btISJaHPf3bXP6S5cuxZo1a9q1eYZhmI5ECLGzmd+zvMMwDNNDhHL6QoiLhBCbhBBbhRDX1FnmA0KIDUKI9UKIHyRrJsMwDJMEDeUdIUQewI0A3gFgCMBqIcTdRLTBWGY5gM8AeAMRHRVCHJ+WwQzDMEx8wkT6FwDYSkTbiagE4DYAl3iW+SiAG4noKAAQ0cFkzWQYhmGSIIzTPwXAbuPzkPrO5CwAZwkhHhFCPC6EuMhvRUKIq4QQa4QQa4aHh+NZzDAMw8QmjNMXPt95q7QVACwHcCGADwL4phBifs2PiG4hohVEtGLx4tgZRwzDMExMwjj9IQCnGp+XANjrs8xPiahMRC8C2AR5E2AYhmEyRBinvxrAciHEMiFEP4DLAdztWeYnAN4CAEKIRZByz/YkDWUYhmkX64aO4bmhkXabkQgNnT4RVQBcDeBeAC8AuIOI1gshrhNCXKwWuxfAYSHEBgD3A/hrIjqcltEMwzCt5HO/fAGfu2dju81IhFAjcoloJYCVnu+uNd4TgE+qfwzDMF1FuUq1PZkdStvKMDAMw3QKRIRumWSQyzAwDMM0gLon0GenzzAM0wiCjPa7AZZ3GIZhGmCxvMMwDNM7sLzDMAzTQxCAbgn12ekzDMM0gqhrIn3W9BmGYRpgEdAtbp8jfYZhmAYQuCOXYRimZyDqGkmfnT7DMEwjuil7hzV9hmGYBljdEuaDI32GYZhQdIvfZ6fPMAzTAOqi7B2WdxiGYRpgdVGePkf6DMMwDeimgmvs9BmGYRpAHOkzDMP0DmT/1/mwps8wDNMAOTirO7w+R/oMwzANYHmHYRimh5Adue22IhnY6TMMwzSA8/QZhmF6CJ4ukWEYpofoFocPsNNnGIYJRU9l7wghLhJCbBJCbBVCXOPz9yuEEMNCiGfUvz9O3lSGYZj20E1lGBpq+kKIPIAbAbwDwBCA1UKIu4log2fR24no6hRsZBiGaSvd1JEbJtK/AMBWItpORCUAtwG4JF2zGIZhskOvTZd4CoDdxuch9Z2X9wshnhNC/FAIcarfioQQVwkh1ggh1gwPD8cwl2EYpvV008xZYZy+8PnOu/8/A7CUiF4J4D4A3/FbERHdQkQriGjF4sWLo1nKMAzTJqweK8MwBMCM3JcA2GsuQESHiWhaffx3AOclYx7DMEwW6C15ZzWA5UKIZUKIfgCXA7jbXEAIcZLx8WIALyRnIsMwTHvpJnmnYfYOEVWEEFcDuBdAHsC3iGi9EOI6AGuI6G4Afy6EuBhABcARAFekaDPDMExL6aZJVEKVYSCilQBWer671nj/GQCfSdY0hmGYbMBlGBiGYXoIrqfPMAzTQ3RTPX2usskwDNOALgnyAbDTZxiGaUg3deSyvMMwDNOAbpJ32OkzDMM0oJumS2R5h2EYpgGcsskwDNNDdNOIXHb6DMMwDSD7v86HnT7DMEwjumgSFdb0GYZhGtBN0yVypM8wDNMAztNnGIbpIThPn2EYpofopjx9dvoMwzAN0A6/GyQedvoMwzABmI6+C3w+O32GYZggTEffBT6fnT7DMEwQlivS73y3z06fYRgmANPNW53v89npMwzDBOGWdzrf67PTZxiGCcB09F2g7rDTZxiGCcIV6bPTZxiGic9lNz+Ku9YOtduMQFjeYRiGSYhndx/D5gNj7TYjkJ6Ud4QQFwkhNgkhtgohrglY7lIhBAkhViRnIsMw3YpF5EqJzCI9l6cvhMgDuBHAuwCcA+CDQohzfJabA+DPATyRtJEMw3Qn0um324pgzJtS1m9QYQgT6V8AYCsRbSeiEoDbAFzis9w/Avg8gGKC9jEM08V0QiEz07ys2xqGME7/FAC7jc9D6jsbIcSrAZxKRD8PWpEQ4iohxBohxJrh4eHIxjIM0z2Qmmw869Gzy7xsmxqKME5f+Hxn77oQIgfgBgB/1WhFRHQLEa0gohWLFy8ObyXDMF1Hp1SudBVc6wKvH8bpDwE41fi8BMBe4/McAC8H8IAQYgeA1wG4mztzGYYJQkf4Wdf0zXtS1m0NQxinvxrAciHEMiFEP4DLAdyt/0hEx4hoEREtJaKlAB4HcDERrUnFYoZhugLtQDMv75jvM25rGBo6fSKqALgawL0AXgBwBxGtF0JcJ4S4OG0DGYbpTrSzz7obdcs7nU8hzEJEtBLASs9319ZZ9sLmzWIYplfIevRsueSdbNsaBh6RyzBMW7A1favNhjTA1Xnb+T6fnT7DMO2hUzT9LvP57PQZhmkPHZO9Y77PuK1hYKfPMExbICXrpJX7Pl2p4pndI02vpxfLMDAMwySOnb2Tkh/9xXP78PtffwQjk6Wm1tNzBdcYhmHSQDvQtKLniekKLAKK5eZ6insuT59hGCYN0tb0k+ootgwDu8Dns9PvdL750HZc/+vN7TaDYSLjOP10PGka62enz7SdB7ccwgObDrbbjI7k4GgRZ//dPVg3dKzdpvQktgNNOdJv1lHzdIlMpqAOmHkoqxwcm8Z0xcKekcl2m9KTpB3pU0Lr78npEpnsYhFlfkRjVumUPPFuhRLS3OuR1PnlMgxMprCs7miI7UBfzFX2+m2hUzpyu63gGjv9DsdSsw8x0UlbXmCCSXsSFWccQLPyjvG+C5oKO/0OpxOmm8sqZDuFNhvSo6Q9OMuRj5JZj/rU3MoyADv9DsfijtzYdEzBry4l7eOv8+uTlHe6QQlkp9/hsLwTH8cptNmQHqVlmn6TiQ4s7zCZokpAtRtaYhvgSL+9tC57p9lI33jP8g7TbjhPPz6UUEcfE4+0+1SSWr95fXVDU2Gn3+Fwnn58qinLC0ww9ojZlKLn5FI2zXV2fmNhp9/hWBZHqnFheae9pD1dYmLyDo/IZbKEzN5ptxWdCY/IbS/pF1xzv8alGxy9CTv9Dofz9OPDmn57cQZnpbX+hAZnkf/7ToWdfofDkX58tKxg8QFsC51Se8eUd7ohwGKn3+HIPP3Ob4jtQF/AVT58bcEekZva+t3biQtPl8hkCpZ34uPUW+fj1w7iavpVi/Ddx3agVAnuAU6uI9d43wVtJZTTF0JcJITYJITYKoS4xufvfyKEWCeEeEYI8bAQ4pzkTWX8YHknPknVW2fiEbej9UdPDeHan67H1x/YGrhcUn0GZvvohmutodMXQuQB3AjgXQDOAfBBH6f+AyJ6BRGdC+DzAK5P3FLGF4sj/dhwnn57idvROjFdAQAcnSgFLpfGiNxuEHjCRPoXANhKRNuJqATgNgCXmAsQ0ajxcRa64ch0CFx7Jz6cp99e9FGPevzzOQGgcfmR5FJyuytPvxBimVMA7DY+DwF4rXchIcSfAfgkgH4Ab/VbkRDiKgBXAcBpp50W1VbGB9b048OllduLXfAu4uCsnHb6DX6X1E3dvGl0Q1MJE+kLn+9q9p2IbiSiMwF8GsDf+a2IiG4hohVEtGLx4sXRLGV8qVrEMz/FxBkRysevHThlGKKRE9IlNTpvaeTpd0NbCeP0hwCcanxeAmBvwPK3AXhvM0Yx4WF5Jz52nj4fv7YQ1ynnRUh5x3K/xqUXp0tcDWC5EGKZEKIfwOUA7jYXEEIsNz6+G8CW5ExMltU7jmD1jiPtNiMxstSRu3bnUbzhc7/FWLHccNnvP7ETH7zl8RZYVZ8sTpf4wKaD2LB3tPGCXUBc+UXLO42i7nRSNptaVSZo6PSJqALgagD3AngBwB1EtF4IcZ0Q4mK12NVCiPVCiGcgdf0Pp2Zxk1x282O47ObH2m1GYmSptPK24XHsGZnC8Nh0w2U37R/D83uOtcCq+jgpfdk4fgDw2Z9twC0Pbmu3GS0hbkdrIXRHrvs1Lq7Syl0Q64fpyAURrQSw0vPdtcb7v0jYLiYkWcrT186zEsKgqtX+m1UWC66Vq1ao49cNxM3ecTpyW6Ppm34+Q/FBbHhEboeTpVGlOpui3CitAtLuds/4pbffbjtMrAzcDP347cYD+OzP1ie6Tns/I+6u8vkNj1NytXeM99k7NZFhp9/hZCla1c6zHKKYjWW1f/KXsJry0YkSbl61rSU3VovSqy/fDKs2DeOHa4cSXWfcEdF2R25DTV+/Jpe90w3yDjv9DiftSoVRsOWdUJF++yPasHn6v914EJ+7ZyN2H5lK3aYqUaaePDRVosTTFeNmT+nFG+fpJ9OR23NlGJhsk6UMFB15hYn0s+DcnMFBwXZUlHeqtCAEl09A7T+XXqpW8jJYMwXXwvwuqdo7bnkne+cmKj3l9LvhhHmxQkarrUD7qjDO0bLk+IJ2npOw2R1JZYGEIQtPQH5QCgkDVkynbJfEblXKZg/m6XcN0w1KsXYiWaofoyPUShhNv4WOtL4N4ZxC2MgyCaoWZbK+fzWVJ5B42TVhJc00OnK7wev3lNMvlqvtNiFxKKGGnQT6IguTvVNNKAprhrB5+mEjyyRo99NPPdKQ4+Le+PV5aF1HrqnpZ+/cRKWnnP5UFzr9LEX6UbN3gNY40ro2hLxhttLWKmWzlpK8GSV7Q4orv4S9CfMcuf70ltMvdZ/Tt6WHDDgKW94Jo+lnINIP+7Sh72GtMDWrBfQciSu5dcaN9MO2naQkRHfKZufTW04/hUifiPCDJ3ZhNES9mTS2rcmCn9A2hMreyUCxMwrpFOxIvwVeX0fUWaMaMrqOQtxIXJsQtiO3WZtdZRiyeHIi0lNOPw1Nf+joFP7Pj9fhvg0HEl93I8y2nAl5x+7IDR/pt1XescI5nVbamoVUVj/SmFoy7sTotqbf6GYdss+mEeavsxBcNUtPOf2pUvLZO7rTMkznZdK4B420vzXqi6sc4sqwYkZ5SRK2P6TaQluzmrKZRgZT3IGF9g0opKafpLzTDQJPbzn9FCJ9JwpMfNWhtw1kQxLQzjFMpB82AyNNwp67VnXkEsmxC1non/Gij1GSx8C+6UZcZ1R5p/kbVbaus2Zhp98k9sXQhtZgbjIL0WG0gmut08nrEVZTbtU5rraw7yAqaaQGxx1YGPapI6lJcnpxusSuoZhC9k47s2eyVhPElndCpWzq36RpUQMbQsoLzrSKLbIng2MIqyEllSjE7SeIOiI30ekSM3hDjkpPOf105Z12OH3jfQa8vtORGyJ7JwsduWHz9FuUXpqFNNZ6pPEUYne0Rv5dOFvi9hnUrIflnc5FO/2+vN9c7/FopoOrWK7iie2HY287a5p+lNo7aWSDRCV0nn6LZJdWlnuISlIONIl1hu0LSKwMA8s7nYsenNWXT263m4lYf/bsXlz+74/jyEQp1rbJ8K1ZcBROGYYQkb4ti6VqUiD6kDU6dHE7HKOShTTWeqRxvuI65bADxdIorcx5+h2GztNP8rw1M3BnslQFUfzxA1lL2YxWe8f9m3ZghYysW+WMk+p4TIM0ZhmLOziLQp6PuFU8g7ed3LraRU85fS3vJNlwm+nIbTZtMWsduVEGZ1EKTiQqofP0UyhB4G9PluWd5Dty45dhkK+hUzabtNkt72Tv3ESlt5y+kneSbLiOvBP9t81e5K5Usgw4CjvSD3F8s1AzKKy80CqtPQud2/VI4xjEza4J28eSlKaftb6zZuktp68i/UQbrv1I3t5IPwu53fpYRBmc1d7aO+GcTlg5oVmcshCpbiYWWo5LZXBWxFWGnZazmWvTvT1jnRk8N1HpKadfLMtWYFFykXEz0XolSXknA7ndzojcxvtDKTiRqIR1Oq2q/R9WtmgHaQzOcs1IFeHYhm1nieXpm++zeEeOSI85fafDNKlz18wjebPVG7NWcC2SvJMB/TrsuasmFDGGtScL59JLmrV35HrD/86u5tog0nFSQiMaVrMe4+bU3KoyQU85fTN/PCk5pBnH3ayGa+rhWfAT2p5yiGkpw2bOpEnYsQKOvJOuPVk4JvVIo1ZS3Owzu52FjPSTlHe6weuHcvpCiIuEEJuEEFuFENf4/P2TQogNQojnhBC/EUKcnryp0diwdxT3bzro+s5ssEk13mY6JO2LPKYzcUdK7W+N+hqMMolKe0sry9dGh65Vnc5ZOCb1SGNwljsRIcrvnOMUdE6SmyM33s0pqzR0+kKIPIAbAbwLwDkAPiiEOMez2NMAVhDRKwH8EMDnkzY0Kt98aDs+e/d613fVFCLjZqpsak0/jJMM2rb3fbuINDgrBY04KqEnRm9RemkWOrfr4dTeSW6dsSN9Y9EgiccZfJdcpJ/BUxOZMJH+BQC2EtF2IioBuA3AJeYCRHQ/EU2qj48DWJKsmdEpVa0a5+OK9BO6gJvRe5vVcLOWpx9pusQW6eSBNoSMXtOIcgPtycLJ9JDG7GHujtzwvzOv46DO3MTkHfN99k5NZMI4/VMA7DY+D6nv6nElgHv8/iCEuEoIsUYIsWZ4eDi8lTHwm2u0mkJknEhHbszoKbN5+mGqbKYw2CcqYTNSWi3vZOGpzUsatsWVJ822Huz03a9xydoTdbOEcfp+1cl891wI8UcAVgD4gt/fiegWIlpBRCsWL14c3soYVCyy5RP7O6OBJHUBN9WR2+TEFFmbI7caI0+/vSNy5bYb1tNvVZ5+i2SkOKRRWjlu9pl5fEoBbS2NjtzsnZnoFEIsMwTgVOPzEgB7vQsJId4O4G8BvJmIppMxLz6WVTvtnGswUwY6cquWHjcQV94x37e/OUaqp2873FRNamCD+7Xuci3S2rNQhK4eVpMBiu86Y6ZCmiYESYmOph/RMO96/FbawYSJ9FcDWC6EWCaE6AdwOYC7zQWEEK8G8A0AFxPRQZ91tJyKRTURpxn5J9V2m4kC7UEmidTeaX9jrNpOP0z2jvpNGx9RwvaptKyefgb6OeqRxpOZS9OPcKMzA6xypQWafq/l6RNRBcDVAO4F8AKAO4hovRDiOiHExWqxLwCYDeBOIcQzQoi766yuZfhp+paVvJNMQt6J+8ictZogdsG1CLV3sjBdYmNNX7/2rryTxpNZ3CdVc9mg7J005J0sdrJHJYy8AyJaCWCl57trjfdvT9iupqlaVHPxVFJw+s1onc1OuJ21PH1tQhhNP6kh8s3g5OmHi/RTT9k0HCsRQYjkJvtpljTGEMTNf3fJOy3oyO25SL9TaRTpJ9V4nUg/+m+bzf9OY7BZM2gbws2Rq51IqiYF2xBR3kn7/pS1jnmTNKdL9L5v+DtT3gloQHHr9desx7XOplaVCbrW6VcsqzZ7xyL0q1mzkuosy0o9/Sw0Rm1PmDz9LNSZcfoVgpdLowSB/3Zqt5kVtDlJPpm5O3JjyjuB2Tvqtclr3ZUa3dyqMkHXOv0qSUfo1fH1/LjJyTvqtS1O33zfIFq1KHUpJVqevnrNQJ5+aHkndaefvPyYFGk8mblSISPsrtm8gtpaKh25GTsvcehep69u7+bjaMUi9BVyNd83QxIF1+I2yihywO/f9Ci++tutsbYTFkfeCV9wLQt5+uFnzkrXVvf5zJZzSWOsQtzECnPZoP6jNFJtM3ZaYtG1Tl938Hh1bz0pelJ37HZ25EaJ9HcfmcSuI5OByzSLtidMPf1s1N5xvzZeLuVIP8Oafhod74lo+gE/dPL0m430jfddIPB0rdP3eySvGpp+Uo+pzXRwOQXXktD0g9dR8enYTpqwmj4ROfVs2ujdQnfktqjT2TwUmdP0ddpqknn6ZvZOhP11D7JMP2UzazWumqVrnb6fQ5WRvrDfJ0Ez0bpTWrl5p98417y2LEXSmJp+0E0oKyOJw2bltGzmLCueE2wFacg7cQ9ny1M2zffZOi2x6Fqn79dJaso7iefpN6Hpx42eouTpl6tWqPz5ZjBXH3SDyUqqadgRsK0aPZzpjtxUBmfF1PSN4C2onaVTeydb5yUOveX0KXmn30yk32z1xqxF+uGrH5qyVKomBRJV3km/tLK7rWYFU45Lb+asaL/TMm1Qm3Y0/VjmubbnXWcn0zNOX6Ysws7eSartxplcomoRth4ci1S2wA9X/nBAaySilmj65vrDDI8H2l2GQb42OnetLq0MZMu5pDEPBRBf5qsa13ErNH0TTtnMMN5ZqXRj7U9Y06/G6OD69YYDeOcND+LAaLEpW8I+HkdJpWwGV3psQKSfGXknap5+6pG+8z5LHblhg4uoxM1/JyPSb32eflOrygRd6/S988/qi6i/kLC8E6ODa2SyBIuAY1Plpmxx5XUH+POKFd3GePYY2wwxUlL+pv1Ov3HKZrjlmiUrN0Mv7myZ5Nbr0soj7G7VIvs61sdprFjGl+/b7Gp3iXXkujT9zqdrnX5NpK8+25p+YpF+9GhC5xYXy9q2eNs2HX3Q9u1jEadAUATc8k5ABJYR52Z30IYdnMXyTsLyThOavnL62sk/svUQvnzfFmzcPwYg2VG0VsybU1bpWqfv1fQrHqef3By50aNo3VCnK1UA8SP9sNM/VqvuG2BaWETQhSHLlXCafnsHZ4WVd+Rrq8Y5ANnqyI0yHiTaev23EeZ33o7cUtUtYcYd+OVH3GqgWaX7nb7d0ao1fT0iN5ntxHn0r9gNtLkIPGwZBu/TTlpYFmFAR2ABN5g05iquZ8+nf/gcnt094v/3iPJO6qWVQz65tRrzVKaXvRPB6fvIOzqQ0jeBJCcYYnmnQ3AagzfST7ojN/qjvzezJa4zCRsp6X0PUwitGapEGCjkG24rLSfiZaJUwe1rduPhrYcC7QhbeydtP+xyVBbhyltX43uP70x3oyEw22dag7OiHFszZdMbODmRfnJPk/rnQkQ0NKOEmkSlE/FGt7oR9CddcC2kLuyyzeMQ087Tb1VHrkUwtNYAp98ieafS4EnK8jwF1qPZGklhcfV1EGHNzqNYPGcg1W2GIa2+hrgF5qoEDPa5UzZ1IKVvAu4bSrORvpQtBTjSzwxfvHcTPnDzY/ZnIqpxxl5NPyltMk4pBW9mSxKRftD+6O0F5c4ngSnvBG2rVSNPddRXL1U17OCdODf2OLhkL0vaXWrnLDO2Len0NcTtICVyBllqJ6/7kCq+kX7z8o4AIITIlOwWl46P9A+MFvG1+90lg/1S3yyP00+s4FoMvdeb2RJ/ukS3HFCP1kX6FD3ST9EmfZzr3YBCl1YO+UTQLF65rlKl1DOuwpCWvBPXMVctQiEnkBO1iRr6JuA6lk1e6wRCTggI0RXqTudH+rc9udt+T56oHqjV9BPP028ie0eTzOCs+st5+zfSomqZmn64PP00o2cd/ZUrDeSdRpF+jLTcOHhTWUtVK/UBdWFIb3CW/zbC2JPLCRTyuRpnr2XdJCN9i6DkHcHyThYYLZbt9345887gKdkY7JmzkurIjREFejs5Yzv9kNkeZTuzIV0HQobWGuSs3PJOevbo/a233/qQhY300/a/rnEOVbdG3U7cN6ME1xszFZSIkBNAISecrB37VWn6lrl8c3ZKeUeK+t0g73S80zedi185ZUfakJ+dgmvJbD9OPX2vE4rbkMJ2sLVsjlcjqyIL8k6poqNAf0+lz1lDTb9Vkb6x/ulKcH9EK8na4KyqJeWWQk44kb7lPteJavqQor6QHzqernL6utOrajicqifaS3pwlhUjCvQ6xLgF19yPx401/bSrbFYtwkBfXm2r/QXXKlZwtBy6yqb6cysHZ+mBe2k/nYUhrcFZcTNsLCJb3vHm6TuafnJOHwTkBJATLO9kgmlj5KfWcN0dT/JVXzv9WSjD0OqUzSYHgYWFCKEKYWUleydqnn76kb7zXpfoqNcf0UrctXeSjPT934f5XU4I5HPCkPC8mn68dftvjyCgO3Lbfz6aJZTTF0JcJITYJITYKoS4xufvbxJCPCWEqAghLk3ezPqYzkW/d2fveCN9pekndPLsKptROnK9g7NiO33zfVCk3xpNv0qEgRCavqsvIs3snQY3O7Ij/eCLOY0SvX6Y7UBH+mmn2YbBPJX/8fCLuPLW1YmsN259HIsIeQH05YR9bksVd6SfZO0dIth5+ik/7LWEhk5fCJEHcCOAdwE4B8AHhRDneBbbBeAKAD9I2sBGmDVenA7LWk1fX7B9nuHbzRJ2gI+J1wnFlTjced0BTr9Fkb5FhEGVvZOlwVl1I31j25+841kcnSjVWa41fSLmOZwuBz+ltBJvX8NTu44mvt44mn4+L2oifCdP39xOc3YSoFI2Rc+kbF4AYCsRbSeiEoDbAFxiLkBEO4joOQAtb6F+mr5fFUftAPoSrr0TpyPXeyEnkqcfsAozlzmtx1M9u1J/mMFZKckFXhrKO4YdP356D57e7e/MHHknYQNr7HHeTzdIN20l3nNUCiimF4WwT6peiFTKZs5J2fSmZkeRpIbHprF+77HA7cnBWb0zXeIpAHYbn4fUd5ERQlwlhFgjhFgzPDwcZxU1mCMWvSceqHXK/al15EaRdzyRfuyUzXDaeMW1XKxNNUTvw0CYwVkt1vTrdWB7v9bRdb3l0h6cVXVF1NmRd7znKKlRwq4O4oi/0ymbVU9nfZzsnZse2IY//s6aYDt1GYbO9/mhnL7w+S7WrhPRLUS0gohWLF68OM4qajCjOP3enELNOxrVW52vWeJ15CaVsum8D1OGwW/bSaFtscswhBycla7Td+u9XrzHrKgcrUlaJQga2VPMlLzj/lyuUmI3wJzQ2wi/vqpFyOdkR653UJY+Xu7MoOD1jRXLGCtWApfRZRh6pSN3CMCpxuclAPamY050zI5cO2XTp6PQO4lKUicvXj39hCL9sNk7Pk8+SaNtcUor19+Ou6M9FXOUDWiIKMYAACAASURBVMGRvteJ+0X6YftNksCvIzcTZRh89juJaN8iQiEX/XqUI2QF+lwpm+7XKJH+dMWyj7cfpFJEpbzT+YRx+qsBLBdCLBNC9AO4HMDd6ZoVnnLVcnRkXXQpINJPurSyOZQ/bMOtzd6Jt+3wefq1EljS2E5f5ekHTaKSZGZFEKUGA5y8Tnzax+ZWjR4GPPJOpiL92h33O1aR12sB+Zyw30exJyegIn33cfKtvdOgjZUqVuDTC0FG+rle6cglogqAqwHcC+AFAHcQ0XohxHVCiIsBQAhxvhBiCMBlAL4hhFifptEmpYqFWf3uGu5+BddseSelEblR1llbhiF8i//mQ9vxidueVtsLGekb20srbdN7fIOmS2xVR26jeQS8F7BftGcu08o5hqc9KYjtxM8ZBkXGoddLsnCafh/ld3k1Irdqn2N3wBelHLTel3pPL3JGOKFSNtt/PpolVJVNIloJYKXnu2uN96shZZ+WU65amNlfwNHJst3pFeT0+xIuuFb1OAUduQRRE+lHMOXpXSN4WqXMhdX0WzHhtl6tzKoQNUXl6tmTph8Nk72TE44NxUbyTjvy9DMQ6fv1ZSSRwaOzcIBosknVkk644ErZdN/g9fWQC1EvRzv76bKFQfWkWmOnuqw73+V3wYjccpUwa8AtKfg6fZ2nn084Tz9GJkozk6gUy1UUK+5oppALrvNtRt1BEXgz6H3IC7guRj/CylLN4gzOqp+VozVlwD96bcUN07HHdPruAmLtxO/hMAl5h+AESdEKrklpp5DLGeUX/PP0C7lcw8BCS2nTVf+nF/lzAaBH5J2soyN9+d591zffezX9NOSdsE6hmZTNYqWKYlk2Tn2h5HMiOE/fcHrVlJyIvqnmcgJ9uVzoKputydP334ZF7iczv47cuLM7xcFvcFaparU9Y8Rvv5OI9C0yNP0Iu2hq+t6OXG/KZr5BQAS4I30/9MxZuS6puNYlTt9dw90v+q7V9BOK9Ml9g/neYzswVQrWO5uppz9dtuwoy4lmghu2+yaYVsqmfpxWj91BtXcScKSrNg/jhX2jgct4o0AvRLA1ZaB9Hbl/9v2ncN3PNriOi5k+mnahvEakJe/E1fSrlsym6cv7VNnUKdTKvEKucXRu3mD9MAdn6fV+7Htr8M8rX3At9/6bHsU3Vm0LvR/touOdfqniRPoluzPHcHI1E6OnU3ANANbvOYa//+l6rNp8MPA3NR25ERp8sVJF1SKUq5Yrmglahd+TT9LoiyGnUumCIv0koue//8nzuOmB4Aus1KAMQ5UIhbzj9PUTlHcZ+32Cx27/sSL2HysCANbvPYYtB8fcI3KNqLPdEo9/R277In2psauCax4JzztdYiGfRKSvyjBA2CNyN+4fw+YDY67lNu0faxiIZIGOd/ouTd/O0zcvVHf035fwxOjmevSELhPTDSL9Jgqu6c7G6YplXyiNHmGrPjfBpHFuQFBOP8ge+WpGTlGZLFUwWQoeUOOdWMOLRYRC3tT0a40x7UvS6X/6rudwzY+eAwCMFSuYKlXd8o4R6bd7nly/3U6mIzeepl/VI3LzOaPmjlvm0asr5HON8/TLwdk7BEfe0auaLFVdT/REhKlyFcemyr7ryBIdP0euqelX/FI2yRPp23nBCck7xnrGlbOf8okYTWo6ciM0eO0MimXpJHSd7yCH5J5oJt2UTVveCVF7py+Xi33znSpVGx9n+5HfX6uvlXd8RuSmNKZgeGxaPaERRotlTJWrKjVQOhbzBhSUCdUK/AdnJZOy6Tj9iL/zTqKipTwt7xhJDo2ebp1I33+fLDJG5KrvpkpV15NhuUqoWtQRTr+jI33LIlQsMvL0feQdTyPQw7cT68g1WuvEtIw8/WQCE6/cECX61pF+UTkJXf0vsCO3FfKOqenngjV97TwL+XjD2nVU1ajvxFtu170O+Wp25PqmbFpOu0myDMNEqYKpchXTamDQVLmKqiVvhIBnnogI7ePYZBkXfflBbNo/1njhkOhzK4xs5HpSSBSIgLyIpunrm7XwyDvlmiqbpvTZKNIPoekbdhIRJtX50+j3ptMnIrzliw/grrVDofatVXS009cneuaAW9N3Rfqex758TiAnEiy4ZrSTceX0Jxt15Hocb6RIXzUuLe/khFCPneE6ctPO08/nGmv62gZzcE0U9L43Ps5Oe/A+2ZmRoLPe+pG+tDWyqXWZmJaSjpYEi6WqkptEjS1RcvV3HZnExv1jgVUjo+KbvZNIGYbomr6rneVqyzA42TtyuUKIAG+6kaav5B0hAJDT/kynX7SdviM5TpWrePHQBDYfTO4GnASd7fTVibYjfVWG1tX5RrWRfk407twJizkgS0f6UeWdaCmbTqTvpJI1yN5pQcE1vQ9CSE0/TO2dvnwu0sA0jb7AGj5RGWWJvRKP6Tw0vpq+Wq4vhDYchfFp2SehC31peUcnGpgOKMo5m1D9HBMNbohR0OfLNWI4kUifDKcf7tg6T5RQ9fTdT3PewVmN+ruIyH4irHsjI9jXGQH2E+ZUyVlef6dv4oATlDR6Im01He309cka7MsjJ/yrbJq15AHD6SeVvUNk5/7bTr/BSfY6oLBPHZblNFAZbZAd6QftTisiffMiK+RFg+wd+dqXz8WSd/RNtdHNtRxQc8iJ4I2O3AB5py9EFkhYKlULxbKFyVIVo0oOkPKO05bMG1oUSU53bk9OB3dyR8G39k5iBdeiDXW1+45yQs6cZV/f3jIMcvlCLhd4rZuOfnhsGkNHJ33tNKdLnPQJOnRbLFUs+/tJ1cfXKLGj1XS009eOpS+fc0kK7loz7s5dreknFfBalhOd6Y7coAi0apErYurLh5c4zAYqNX11E2uUp28ej7QHZ4kQg7MMTT/OTWgqZATlnkrTbY+vph8g75gVHZtFR+HTFcvWgItlC1XLqTppPnVEyZTRDibJSN+vTz75wVlhNX35KlM2czVZO94qm43Smc3j/E8rX8AV366dCpIgnyz0dIl2+1NP2/q9Rp9T/dTVKMus1XS009cNry8v0G+kCboKkXmdvh0ZJxfp6wFfYeQdr/OJ4kzMm8l02bKzPRpV/2t1nn6jwVmmI41zHqbsSCvY8bhlLY+kZtx4NH6RvsvWpJy+EYUfHJu230+WqoamH0/eSSPS93sSTa7gWrQCiE5wAVeWmLfOUtg8/ZLn5npgtFizjO7IFR55R46Xkesulmqd/mQKUlsSdLTT1ye4v5BDXyFXk71jjtgzI/1GkXEULMvpfNN39qAI1Ot0ozgT08lNV6r2oJFGNzG33NVchLZ9eBxLr/kFnhsacX1vRlaFfC6wxo/e30JOxMrT1ze/UtUKTGf0m2DHz15N0Ihc6Tyi2+qHy+kbTmZiumI/NZpEuVGnE+n7dOQmMWUiAVpdi67p16uy6e5/aJSp5z3n49MV305/e7pElbmj8ZMa7Uh/Wss8HOknhr7L9uVzKBi1tc2SC2aVzXxO3q3zCXbkWgRD3mkc6XudVH8hfK66GV0Vy1rT1x259X9XcckcngZtET5w82P4/hM7Q9nw6LbDAIBbH93h+t7J05djIcJU2Szk4+Xpm1k7RR/nc+Wtq3Hv+v2uffU+eZD6WV+Dgmv6ptQX01Y/xg0ncGDUifQnSlVb0zcJmpvAy2QKkkJag7Pcefrh+7UAx+nrOvjaRn2swhYj9O4HEWzN3v4O0uELn79P+zx1jmY80u/owVleTd+bsjnQl3dV2dQ5wUIkp+lXLR95J+Ake51ufz4Xeki72bDcefrBkVJQR+5osYwndxzBkzuO4LzTF+ClJ84NtEFPWDN0dMr1vR2BhUjZdDJi4t18zeM7Wapg9oDTjIvlKn6z8SBOXTjTZYM3M8M30g8orSyfSpKSdxz7D3gifXNfNFEqo2oHk2Tnob+8k5Smr2bOivAbQPfN+YxpsMflOMsFa/q1x2msWHafB3Ly9Ankan+hIn3W9JOjZDt9gf6Co+kHRfqALBWQbPaOivSLISJ9yyfSD6lxuDR9lSsshFCafpDTt+rOXTsy6aSY3fv8gYY2jEyWAAB76jn9qJp+nI5cMz+65N4ffcEdnigFTh5jar6aUtWqm8+fWqTv0vQrzUf60ylE+inJO66CayHbgZmyWfDLdPJq+rngfiO//Rj3zJdLUPIOVKQf0unrJwLO3kkQfTH053NSv/foev2FnGuAju30k5R3LEJfQa43nLzjjjCjZO+YEY3O07flnYBrsFIle3II77ZGjBGER5VDD+KouknsOzblcix2ZKULrgWVYTA1/RinwS9VTqNvYofHp13RvZmzb9prDs4CfJ4IjP4homgyRD2Ja7yOpj8+XbHnYjWJUjrDjvST1PRT6sglijE4y0jZ1OdOtwEhjOwdow8v6Fr3e2IZ82jwlgVXwsSUqemr4+zbkWs/+XOknxi2pl9wp2xWDaevrzvT6QuR3JB6M9Kf8GkAtTZLg2YoJ9xfaDzJg6Ym0rcQSt6pWoTBPv8Jy0cMRx/G6evlLZKVBs1tALJjrlEZBnPAUzMpm4Cf05f2HZkooWJZtmPw3oSccQXuS8Ar8ZgDyUzb62FZsgLql3+zBe/7+qO+y0y4NH3H6RfLlsoukzbr+1EpQpqtk86aYPZOSpE+UfTSytoU+UQpz4m+Lmb05e3zHHZErt9+jPlE+uZ1FhTpz+zP24GH9geT5WpiykISdLjT92r6bnlnwJBO3PJOMjPg6Dog3tm4guUducwMNYq4P2bKZrFctasNNhqRW7acSN8bfeqoZM5gAUcmGjt9c5l9I47DcnWwNaiy6UgmMTV9wzF7+09GPPKOPs61g7PkqzfS9+bqmzcooPHgtptWbcNFX34QG/aO4oV9o74XuxnpWwSXfpxTiQYAnMmBouTp687DBCWFNMsw2NMlhmwGrpRNT6Q/oy/vm6cv1++/Ab8nlhp5x/gpwZNIYDj9/nwOJ8wdxGF1jehIn8h/DEi76Ginb2r6fXlhXxxm7XxzDk3dAHIimZGp3olZNEE1YbyRfpRo1y3v6Dx9gVwuOAKtWhYGC8r51UT60kmesWiWS9+vx9HJMs46YTYAd5Rqdpz1NaqyaWTvxOvIdS5K70C4Y2ofjk6UUKpY9nH29mVo55H3aOjeSN+r/Teyd/3eY9g2PIGho5OoWOSSzzQTHvngpHmD9vtczilsZt+wIsg7kyl0Hnrb1uyBQiJlGHS1TAB2nfqGvzGCC3096zYw2JdHxSIVjHnPm//6fDX9afc5Ixh5+uSRF1Wf0lSpisG+HBbPHrAlO1Niy5Ku39FO387T94zItZSDN9O1LMtpYEnl6VeNiNVESi/+69eRiCnvhJWa3PKOytPPham9Qxjo849UtaM//bhZoSL9kckSlh43C/35HA6MOU7fjMD68rnA6NSM9GPJO8Zx8N5gR6bkPlQswuGJUs2sarYNxmA9E2/kVyvvBNurJ0bZcnAcAHBwrHawj9fpn7F4lv1ejxgHYNseRd5Js/aOZvZAIbFIvxBV0zdHftsduSqQss81GUFI8Hnz1fRrIn39RA0A/vJOsVzFjP48Fs8dwPC47Jw3b7xZyuDpCqdfU4ZBOX3XzDopdOSaOdxe6j3O6ahtsD96pK9z0vsLOSNPv3Fp5YpFdSP9Y1MyPW3xnAGXvl+Po5NlLJzVj+PnDuCgkWPuzd4JMzgrn8vFktn8StpqzKeVY1NlzPDMn6zR2/Vu3jvK13tjb3SudN69Xm7YyM7RjE9XcdysfvvzskWz7femvDOjjiQXhHZIpYqVWHE9bwAze7CQzMToFKfgmnw1UzZNTR+Q15i3impzTh92lU2L5DHWT/empj+jL4/FswcwPKqdPkf6iaMzMpyOXK3pyw4xs7aLORCk0aQjYbEdQsE5jDqPvV6uftmO9OVy9py9IezRA0HmzehDsSJr73hLK1sW1eiXFcuJ9L3a9shUCfNm9GHhrH5MeCaG8EJEGJksYcGsfhw/Z8AVxZqP3X25XPDgLHUu4pa4nipZznH2On2PnDKz399xmk+AgHPevJE+2U5fn6f6dlkW1QzjN50+kRxINDFdwULD6ZuRvu4wNG2PVGVz2owuk3E03qY5JyGnb5aSDtsMzNr++kZsa/q+kX7w+oOcPpHMwCJAFlyDUHM5OOevaHecVzHYl8fiOQMYU2WzOdJPgZIh7/QXzBG58g6fM7J0KpaTKZBLaBIVP01fR3D1OnP95B1tXyN0A503o6+m9o6+GD763TX4qzuf9WzTQn8+h5yo1YePTZYxf2Yf5s/sA4BAXX98uoJylbBgZh9OmDvoGk1qXmS6ZEG9G5m8WcV/4iqWnUjZmynlnbnIkUj8nb4+hwvU/nslLv0zXSMm6CZ1eKJUcx7N2jrX3LUOH/72k/LpatDpvF16XD15x/8pJYjJUtUuNZ6Uo/Hu8+yBQjLZO4CdqRQ6T99IxXQ0fXc/WaVqOZp+g0g/SNP/9F3P4X/e8rhxnTkduQs91/mUkneOnzMAQN7sJ6adc5GlUbkd6fR1zWpT0y/kzJRNC/m8uzaHZZGdKZAT9RvZwdFijeZaDzOHW7NgpnJGdZy+TimbYcg7QLjH22K5CiF0pKXz9FXtHUv+/aEth3D/xoOuaF9nLhVytXXuR6ak01+o7A5K29Q3hPkz+5XTNzR9M9JX+1QvV19O86hkqRi+Y6pcxfyZ/jfXY5NlV8eo4wjc+60Pg3ZoLz95HvI5gXV73JOPOOm/jWUIv2JdOtInIvxm4wE8tu0w1u055hr5fPL8QdtOnY0FmJFruINERJgoVbBYOZ6kJAU9LadGt78k1qudctjbmitl0yd7B5ABlDd7p949xbsf82f2YXy6AiLCbzcexNqdR/HUzqNyRK6SUSdLVcweLKA/n3Nr+irSB4Dh8SImjXORpVz9UE5fCHGREGKTEGKrEOIan78PCCFuV39/QgixNGlDNd98aDve9Pn7VdSpNP2CcMk7FdVp69b0nZztegM2iuUq/se/PYzP/GhdzffvvGEVzrn2l66pz6qeR38AdgRQ79Fa2zNoR/rhtGJARvoDhRwGC3kZ6Vuq5KuKmJ/fcwylqoWjk2VsG55wtmnJsQR+pYxHJqW8s0DZfdRMyTw2hbd88QE8purt7FdObeFMqemPFZ3JyZ0yDM5N8K1fXIXbntyFd//bQ3YHpz42MlKT+/1vv9mC//WtJ0FE+Jd7XsCf/udaAMDf/WQd/uoO91OL/H0FcwbkRTcxXcGlNz2Kl/39L/HvD27HyFTJJZfUk0i0/KZv3HMGC3jpiXPw9C5ZSO6pXUfx5i/cjx2H5XG0q0EGnCdzH+VvhB3p7zw8iUPj8klgfLqCFacvsJc7Ye6g7eBzQtjByUAh52rDjSiWLRABi2YP2MfJyzHPk9yqzcN465ce8O17+METu3DOtb/E1+7f6nKag315jBUrdmDxD3evx0v//h5c/YOnfO0iInzk1tW4/lebPN87KZvX/3oz7lyzG4Bsk++4fhXuXb+/Zl1mjSe7DLWRIw/I6N2yn9CiRfonzZuBsWLFPl8AMFqsGCNypcQ5qz+Pgb6cq8zyjL48jp8jA46DozLS105//7Ei3nnDKqxct8/XjlbS0OkLIfIAbgTwLgDnAPigEOIcz2JXAjhKRC8BcAOAf03aUM35SxdiZLKM257c5Sq41l8Q9iO81u9N5161nAhKyj61677n+X04ND6Nlev2ufTqO9cOYfOBccwZLOCG+zY7Q72t+k6/nqZf8aRsamnoyu+sdo3O9LL14Dge3DyMgUIeg305pemT3fFHBKzZedRefu3OI65t6uPhdX7HpsqYN6PffkI5YkT633zoRbx4aAI33LcZAPBfT+7CjL48zjt9AU5QjXufcnR2ZCWcDrY9I1O45kfrsH7vKD7+/bUYV1rnL9btw++ccRzyOYEjEyXc9MA2PLh5GHes2Y1vPfwi7nl+P+5csxvff2IX7npqCC/sG3XZPFW2MKNfHoefPbsXa3YexdwZBXzt/q3YO1LECXMGccGyhQBgD+Dxdiz/cO1u9OUFXnXqfHu5V582H8/sHkHVInz5vi3YeXgSN6/aBgDG00uA01fnT990XnL8bAyrdmSeGwBYsdRx+n35nBHpOx253kKCjdCZO/Ui/e8+tgPn/uOv8PPn9gKQDuxLv9qE7cMTuPXRF13LTleq+MpvNmPOYG0toNefuQjDY9NYtXkYQ0cn8b3Hd2LejD78/Ll9eH5P7TSNj2w9jN9uPIhvPLgdh8b1TXDCdpSaG369GeWqhf98fCe2HBzH9b/aXNM/ZSYM6HRbfa3p5Ij1e48ZVXVVbZ86h3C6Yrnk2UWz+zFWrNjn6/dffQoApyP3md0j2HxgHG94ySLM6Ms7efqlKgb7zUh/GpOlin0D/veHXsTmA+P44q82tX2gVpiCaxcA2EpE2wFACHEbgEsAbDCWuQTAP6j3PwTwNSGEoDjTIjXgVafOx+vOWIjrf73ZjpYLqsjX4fFpvOP6Vdg/WsTsgQIKuRy2D0/gHdevwp6RKZy5WGZJ5ASw+sUjeMf1q1zrPjBaxOI5Axgem8YlX3vEHjSzd2QK5546H3964Zn42PfW4m3Xr0K/MQagv1Dr9D95x7N25GGiO4nMxg4Aj28/gt/98oN2IzEhAC8emjAGneWxcf8Yth0cx2nHzUIuBzy9ewQb949i2aJZODZVxufu2YhvPiQv5KGjU3jNaQtQyAn86Kk9eHjLIXvdhyd0pC817et+tgFfuW8LABmdLpjZhydfPIK3fekB7Dg8iQ+97nQsmNWPl540BwBw6U2PYvZgAaNqbtBcTtTcvC49bwl+/PQeXPiFBzDYl8ORiRI+9uYz8ZNn9sgbdVU+Vv+fHz8PQEbd1/xone30PvQfT9qaOwDsPDKJt5y9GAN9eew9VsRpC2fi+g+8Cpfe/BgAYO6MPnz0FSfhyRePYO+IrBH0lfu24LtGZdAdhyfw3nNPwYlKCprVn8crl8zHfz6+C++4fhW2H5rAgpl9dtmJpYtmAgDee+MjmD/DscXk6GQJOSGlor0jUzhj8Szc98JBvOP6VTg8UcLcwQKOnzuIkckyTls40/Vb3dFuFoAr5OQ8Ebev2Y3fbjzou00T3R614/nUne42uOPwBHJC4FN3Pouv3LcFVSJsH5b7+c2HXsSv1ju1l6YrFg6MTuM7H7kAH/7Wk67tXPyqk/HFezfhE7c/g4FCDgLAdz/yWrz/pkdxxbeftAMIzaHxacyb0YfRYhnv+erDmD1QsNNp/+C1p+HrD8gb695jRbztS6twcKyIBTP7sOnAGN76pVWuAXQ6K86Ud76tzutMdU39yX86Txy6o/h9X3/EdWw1w+PT6C84xRrnDvbhie1H8M8rX8DcwQL+//f8N/xqwwFVWllgrFjBnMECLr/gNHzv8Z34xXP7sHbnUew8PIlzT12AhbP6kc8JfOW+LRiZKtvnYs/IFBbM7MP24Qm89UsP4K/eeTbe86qTG53SVAjj9E8BsNv4PATgtfWWIaKKEOIYgOMAHDIXEkJcBeAqADjttNNimgz83bvPwc2rtsEiwvLj50AIgUvOPQWHx0sgEJafMBsXLF2IZYtn24M+lp8wG29/2QkAgA+/fqnvo+PyE2bjAytOxfq9o66Jpc86cQ6ufOMynLtkPq584zLsO+YUG3vVknn4gwtOsye6/sgblmGqVMXYdP0O0QUz+/H/vXEZZvbn8b7XLEF/IYc3n3U8bl+zu27xtbe99HgsWTgTA/kcjp87YNchv/Ds4zFnoIB5yhFd9PKTMDldwYNbhl379f7zluBlJ83F07vdEefZJ87Be151EhbPHsBH//sy7Blx9u1lJ83Fx99yJm59ZAdGi2W8csl8fPzCMwEA/+3kebj9qtfh9tW7QZBSxPFzB7H0uFn4o9edjnxO4PdeeTJ++uwe/M3vvhTve/Up+K8nd8EiwnvPPQXnL10AIaTc8PJT5uGsE2bjrqeGcP7ShTh+ziB+sW4v3rR8MfryOfxmo7sQnD5Pv3PGcXhyxxFcfv5pWLF0If7ibcuxdXgc73v1KXjFKfPwF29bjne/8iTcuWa3a78A4JyT5+LP37YcJ8wdxLbhcfzZW14CsoD3v2YJpsoVnHf6Anz0TWfgxvu3YvZAAR+84DQsWTADP356b2CBvHNOmovfOXMRzl+6AGcsdlIxlwN4w0sWYfHsAUyWqhBC4KY/fI3tiD72pjOwavMwLluxBBcsW4gnXzyC95+3BGceP7tm7oIgzj1VttFy1arp1F6xdCH+8LWn4dZHd9jSz+vOOA5/9NrTcdOqbTX79a5XnIg3LV+EH3389Vi74yhWLF2ADftG0V/I4Z/e93Lc9dSQvY6zT5yDf3rfy+teV+899xTsPDxpt7+zhMCHXnc6liyYib+56Gy88SWL8LNn92LPyBRyYh7+5M1n4o41u+0nA5MLlh6H15y+APmcwKXnLVFyXx8+8sZlqFiEZYtmYc3Oo1g4sw//8/xTsf9Yse4At+UnzMYrl8zH+UsXYPOBcSxZMMP2GW94ySLMm9mHf33/K0EgVC3CCXMHcNHLT8LsgQKuetMZeGSrdHFnnTAHl61YgnxO4C/fvhwb9o1CCIEPrDgVswcK2HVkEh9705m466khHBwr2tdrOxCNgnEhxGUAfpeI/lh9/hCAC4jofxvLrFfLDKnP29Qyh+utd8WKFbRmzZoEdoFhGKZ3EEKsJaIVcX8fpiN3CMCpxuclAPbWW0YIUQAwD8ARMAzDMJkijNNfDWC5EGKZEKIfwOUA7vYsczeAD6v3lwL4bRp6PsMwDNMcDTV9pdFfDeBeAHkA3yKi9UKI6wCsIaK7AfwHgO8JIbZCRviXp2k0wzAME49Q0yUS0UoAKz3fXWu8LwK4LFnTGIZhmKTpyBG5DMMwTDzY6TMMw/QQ7PQZhmF6CHb6DMMwPUTDwVmpbViIYQA7Y/586bJykQAACfZJREFUUZK2pMwcAGMNl+o8eL86C96v7HGo8SK+nE5Ei+NuNFT2Tho0Y7QQopOG8i4G8GLDpToP3q/OgvcrYzQzqrYZWN5hGIbpIdjpMwzD9BBtk3ea5JZ2GxCB/w7goXYbkQK8X50F7xcDoI0duQzDMEzrYXmHYRimh2CnzzAM00sQUUv+QRZkewHA/erzFQC+ZvydPMuP+6xjJYCPA/gagB2Q0zS+HsCFAP4WgGX8mwJQAVACcEy9vxZAGcBTAJ5X9lgAvg5gP4BptfweyFkKhwA8od5vAjCh3v9QracM4LcARtX+/SWA3wCwlL3nAlgFoKrWXzHs+78AisY2h9SrBeBxAMMAnlPbeLv6bhOA9Wp/qgC2K5v2q88WgMPqPQE4CuAggI3KxiJkbjCpbR1Rvz9svP4hgHG17F+qYzMN4DYAI8a6DwI4oP42BTnHwoj6GylbCMBatR0LwC61Hf23KWP5qtpXgsy73qWO17j6vNVYloz9fEr97ka13kn1O1Lff9VY56/V9ovq/LxfHYeSYfsE5Cxw69V6DgP4AYB96u+HINtHFcC31TnZp5Z/RJ3v7ytbNhrnWx+33yi7ppSt4+rvhyDbN6lz/TyAO9XvHoVs99rGbQB+rs7pIXWed6vv9X5/DzKV8QiAn6p9fEj97TIAN6t1XgfgGcg2RcY+74Zs2wfVbywAfwB53T2vjsF2td/jAP5a/W7aOEebIa/N1xvX8CfU+kbVen4J4KWQJdy/rPZ3FYDbIdvXQwAeVut7p1rHtNrnn6jzdUzZ92vzHEO2yV9AjgeyAHxFverrQF8TE8rWSXXuquoc3AOnff21en0OcqrYk9V+T6jXTwD4NIB/gbx2/1Xt2zEA3wLwTQA/g/Q5+9S6rlHbPFUd10Uef/dvkO32OQCfMpfxvK/xlUH/WhnpXwng40T0libWcTHkQdKTXb4BwJsgG9aHIBvDFyAbSB/kCQaAfsgT8Uu1zA/U92eo1w9ATvxSUL8rQ574X6ptVCCdzqhaZ1nZUFB/G4a8wH4BYKlh37kAfgfy4j9OrUNf/B9Uy00pm/RM2QKykR4H4CQABSK6D8AgZOO4HnLyeQKwTP1+AdxPbXr7MyGd9unquK0C8BllQwly6srVat/vIKLjIC/sQch5kW9Q9vepY5+D49BPVtsuALgfMl96nlovjOM0V9kzoo7DDPW5pPZTO+TnIEt3jyu7T1bbBaQz0QNZtqn16huwWcZboPbpdVK9vkcd53sAe7Kfw5COetpY7kXIi/dUZc8CIvoDSAcESGc7X73/rHqdD+BsyAmGzgTwu8q2e4191VOOvlYdB23boNqfdxHR/4DjlADpaIQ6Jv8L8vgCMkDYDunUjql1rFe/swCsgWwT3wfwOQD/pdaz3zg+n4d0+v8XwO9Dtm2T9wF4J6RzXa+OUUX97T5l88mQ521A7dcstT19bp4G8FbIwEzzCfU6oI7Zh4hoo/rNiLLzecjrbgDAayDPQxXAM0KIPrXMPLUM1GehtnOuOtb6+jwfwIlqufeo75+EbJczAdwKeWN7F2TbvFDZfiKkTzlfbV+39SUAQER71THeoo7/JZBziVyp1vNetc0tABYCeBmAdwP4PSI6CcA0EX1OLTsL/rxX2fqjOn93odo0hBC1k3ObyyXZkSuE+AnkxbIc8gANQDZCfTGWIS+AWXAcUxX+FyvDMEwvUoIMVE20o14HecM6Hc7T75mQN+U85A2vTETn1Ft50o72I0R0HuTj2k4Ar1bf74CMQkbVv2cg75zfhRP5QhnOMAzTC1TVawmOxAnIJ1z9tEeQT3T6aeZpOJLzH0E+eeQgn8S1LPW5oI0mHen/A+Rj4QkAjleG5eBE+5shNd4PwIn0Ax9FGIZhepAipGw0DCl3vQVOfw0gnwQ2QUb5BUjZ8wTIG8R/EdE/1FtxYpG+EOJCyA7Hv1GGTkI6+YOQjyRTkFH+2crIuyA7k0rGajaCYRimeyGf9zphwTL+tkO9jkL2E2qHPwQZ1Vcg+wV3qPd/DGAVEZ0d5PCBZEfkzoPs9BlUBs6EfNw4HrIjIw/gLPVaBvB7AB6EW7t6SYL2MAzDZBmtduQgJR3zhnAG5E1AvwpIX/kgZDKG9t1TkD51PgAIIRYCmENEdSsYJybvCCEGIFOolkBmcujyx0LtjM4eEJA948L4OXk+MwzDMBILTuac7vcchJR0joNMjNkFGWBvAfBnRPR43bW1Kk8/Zm7/bPVagMxxnfLLSwXwAIAVxqtfjv+48fdbAVxq/PafAXxDHbxJyDzaEz2/vxTA9xrYuwIyr3gFgGrAMhaAKxusa6Y6qfPUZ9vmesvp/YYcx3ClsczPIW/IH1ef90Pmfv8lZN7wzea21DJXQaYDzoN8vNwH4B8hO9/3q2O1BsAb1PJfgHwUvQEyDbIM4J88NrwNMu/515DpdmMA3gjZsf8K9bobMig4BOCvffb3CrjHd9wOqWPm1DG6DFJO3GOci+PUtv4W8uKYDRmYPKPOxXYAr0mgvZ6gXo9Tx+5az9+XQuqzSyHTRa9U+z+tvj8bUqfdDPkYv0Etu1ktY6nl9FiLaTjpvk9B5uZXIfVgnVt/TL3/PoA9yo7tkGmcn4LMBX8eKu9bve6CkwN+jrJDQKb/Pgzg0/r6hOxYXAGZkjgKmae+AzIq1dfvTNVWPqrO7z+q9nCmsexuAJ/1XM+zIdvyn6r9fKv6+19DtsHlxrH9FIDPefzF+3zO0XnqvB+ATH/+mrk85HW+CsB34L62rlDLXgF3+/uUsT9v87Z3z7a/ps75pyBTpa+EzHAsqL//EMCu1P1q2hto8iL6ojpBG1Xj1E4taae/Ds5FMgHgCs9vvwqZHnVWgK3XQGYs3aReJ+ssMw15YQ4ErOvtkBfeJ4zvbJvrLaf2cS3kI+AA5CPfZkjZ7UHV2J6BM0DtXyCdwiHPtt4O6ZQeUp/vhXQkK5Xtu9Q5+QzkmIdnIJ2qzs2vqm0uMmz4ifquDGfAz+3K5oNqnXpg3Ea1H343uSugLjrI3PUJAF9Sn38M2Z90BMD/Vudsqzrmz0BGQWuUHTprrALjIm6yvW6FM+7iMQAzPX9fCulg1ym7B+A4/Q2QOe9F9e/DkHnrG+AMRvI6fa3z6huCOdBNtzOdu69te16d24fV9l6NYKd/nTovRdVWblPn7Rl1XkfUei3VBtZBjjmA0TZ0W7kQ8ma2B/IG8Rykk92stmtekyvUeRyHvBHq9nSf2rcbjeP6Y7WuG+H2F8Jz/Feo43AIMlC4R7UXvfxX1ffbAPw53NfWFfA4fbXd59U671Tf6fZ+p2fb+rr8KWRg+Sjk+V8OeeOcVMfv9c20wTD/uOAawzBMD8EDohiGYXoIdvoMwzA9BDt9hmGYHoKdPsMwTA/BTp9hGKaH+H+IPHoVa4o2/wAAAABJRU5ErkJggg==\n",
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
    "import matplotlib.pyplot as plt\n",
    "plt.plot(idf.get_feature_names(),nmf.components_[0])\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [],
   "source": [
    "test=pd.Series(nmf.components_[1],index=idf.get_feature_names())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "investor       0.378036\n",
      "return         0.350973\n",
      "investment     0.317047\n",
      "shall          0.307254\n",
      "state          0.294281\n",
      "accord         0.225353\n",
      "favourable     0.206982\n",
      "treatment      0.179162\n",
      "maintenance    0.176389\n",
      "territory      0.169641\n",
      "dtype: float64\n"
     ]
    }
   ],
   "source": [
    "print(test.nlargest(10))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
