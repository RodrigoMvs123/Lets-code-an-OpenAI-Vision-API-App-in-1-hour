# Lets-code-an-OpenAI-Vision-API-App-in-1-hour

https://www.youtube.com/watch?v=73sALqCscog 

Acron.io 
https://acorn.io/rodrigomvs123/acorn 

Github Repository
https://github.com/kubowania/react-openai-vision-app-acorn 

Acorn UI
… Create Project
Name
react-openai-vision-app-starter
Create

Visual Studio Code
Terminal
git clone https://github.com/kubowania/react-openai-vision-app-acorn.git
npm i 
npm run start:frontend

localhost:3000

Acron CLI
https://docs.acorn.io/install 
Scoop (Windows)
You can install the latest Acorn CLI with the following:

scoop install acorn

acorn login

acorn run

Endpoints [ https://aged-hill-9d54fb24.zvgz4d.on-acorn.io ]

Prompt
acorn login index.docker.io
? Username Rodrigomvs123
   Password …
acorn build -t index.docker.io/rodrigomvs123/react-openai-vison-app-acorn:dev - -push

Browser Tab
acorn.io/rodrigomvs123/react-openai-vision-app-starter?panel=Referrals 

Acorn UI
Image
index.docker.io/rodrigomvs123/react-openai-vision-app:dev

Generate a Share Link
index.docker.io/rodrigomvs123/react-openai-vision-app:dev
Link https://acorn.io/run/index.docker.io/rodrigomvs123/react-openai-vision-app-acorn:dev?ref=rodrigomvs123 ( copy link )

Visual Studio Code
Terminal
npm run start:backend

Visual Studio Code
Explorer
Open Editors
server.js

server.js // we will fill out this file during the tutorial
const PORT = 8000
const express = require('express')
const cors = require('cors')
const app = express()
app.use(cors())
app.use(express.json())
require('dotenv').config()
const fs = require('fs')
const multer = require('multer')
const OpenAI = require('openai')



app.listen(PORT, () => console.log(`Listening on port ${PORT}`))

Visual Studio Code
Explorer
Open Editors
server.js
App.js

App.js
import { useState } from "react"

const App = () => {
  const [image, setImage] = useState(null)
  const [value, setValue] = useState("")
  const [response, setResponse] = useState("")
  const [error, setError] = useState("")

  const surpriseOptions = [
    'Does the image have a whale?',
    'Is the image fabulously pink?',
    'Does the image have puppies?'
  ]

const surprise = () => {
  const randomValue = surpriseOptions[Math.floor(Math.random() * surpriseOptions.length)]
  setValue(randomValue)
}

const uploadImage = async (e) => {
  setResponse("")
  const formData = new formData()
  formData.append('file', e.target.files[0])
  setImage(e.target.files[0])
  e.target.value = null
  try {
    const options = {
      method: 'POST',
      body: formData
    }
    await fetch('')
  }
  catch (error) {
    console.error(error)
    setError("Something did not work ! Please try again")
  }
} 

const clear = () => {
  setImage(null)
  setValue("")
  setResponse("")
  setError("")
}

  return (
    <div className="app">
      <section className="search-section">
        <div className="image-container">
          {image && <img className="image" src={""} />}
        </div>
        <p className="extra-info">
          <span>
            <label htmlFor="files" className="upload"> upload an image </label>
            <input onChange={uploadImage} id="files" accept="image/*" type="file" hidden />
          </span>
          to ask questions about.
        </p>
        <p>What do you want to know about the image?
          <button className="surprise" onClick={"surprise"} disabled={response}>Surprise me</button>
        </p>
        <div className="input-container">
          <input
            value={value}
            placeholder="What is in the image..."
            onChange={e => setValue(e.target.value)}
          />
          {(!response && !error) && <button onClick={""}>Ask me</button>}
          {(response || error) && <button onClick={clear}>Clear</button>}
        </div>
        {error && <p>{""}</p>}
        {response && <p className="answer">{""}</p>}
      </section>
    </div>
  )
}

export default App

Visual Studio Code
Explorer
Open Editors
server.js

server.js
// we will fill out this file during the tutorial
const PORT = 8000
const express = require('express')
const cors = require('cors')
const app = express()
app.use(cors())
app.use(express.json())
require('dotenv').config()
const fs = require('fs')
const multer = require('multer')
const OpenAI = require('openai')

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'public')
    },
    filename: (req, file, cb) => {
        cb(null, Date.now() + "-" + file.originalname)
    }
})

const upload = multer({ storage: storage }).single('file')
let filePath

app.post('/upload', (req, res) => {
    upload(req, res, (err) => {
        if (err instanceof multer.MulterError) {
            return res.status(500).json(err)
        }
        filePath = req.file.path
        console.log(filePath)
    })
})


app.listen(PORT, () => console.log(`Listening on port ${PORT}`))

Visual Studio Code
Explorer
Open Editors
server.js
App.js

App.js
import { useState } from "react"

const App = () => {
  const [image, setImage] = useState(null)
  const [value, setValue] = useState("")
  const [response, setResponse] = useState("")
  const [error, setError] = useState("")

  const surpriseOptions = [
    'Does the image have a whale?',
    'Is the image fabulously pink?',
    'Does the image have puppies?'
  ]

const surprise = () => {
  const randomValue = surpriseOptions[Math.floor(Math.random() * surpriseOptions.length)]
  setValue(randomValue)
}

const uploadImage = async (e) => {
  setResponse("")
  const formData = new formData()
  formData.append('file', e.target.files[0])
  setImage(e.target.files[0])
  e.target.value = null
  try {
    const options = {
      method: 'POST',
      body: formData
    }
    const response = await fetch('http://localhost:8000/upload', options)
    const data = response.json()
    console.log(data)
  }
  catch (error) {
    console.error(error)
    setError("Something did not work ! Please try again")
  }
}

const analyzeImage = async () => {
  setResponse("")
  if (!image) {
    setError("Error ! Must have an existing image !")
    return
  }
  try {
    const options = {
      method: 'POST',
      body: JSON.stringify({
        message: value
      }),
      headers: {
        'Content-Type': 'application/json'
      }
    }
    const response = await fetch('http://localhost:8000/vision', options)
    const data = response.json()
    console.log(data)
  } catch (error) {
    console.log(error)
    setError("Something did not work. Please try again.")
  }
}

const clear = () => {
  setImage(null)
  setValue("")
  setResponse("")
  setError("")
}

  return (
    <div className="app">
      <section className="search-section">
        <div className="image-container">
          {image && <img className="image" src={URL.createObjectURL(image)} />}
        </div>
        <p className="extra-info">
          <span>
            <label htmlFor="files" className="upload"> upload an image </label>
            <input onChange={uploadImage} id="files" accept="image/*" type="file" hidden />
          </span>
          to ask questions about.
        </p>
        <p>What do you want to know about the image?
          <button className="surprise" onClick={"surprise"} disabled={response}>Surprise me</button>
        </p>
        <div className="input-container">
          <input
            value={value}
            placeholder="What is in the image..."
            onChange={e => setValue(e.target.value)}
          />
          {(!response && !error) && <button onClick={""}>Ask me</button>}
          {(response || error) && <button onClick={clear}>Clear</button>}
        </div>
        {error && <p>{""}</p>}
        {response && <p className="answer">{""}</p>}
      </section>
    </div>
  )
}

export default App

Visual Studio Code
Explorer
Open Editors
server.js

server.js
// we will fill out this file during the tutorial
const PORT = 8000
const express = require('express')
const cors = require('cors')
const app = express()
app.use(cors())
app.use(express.json())
require('dotenv').config()
const fs = require('fs')
const multer = require('multer')
const OpenAI = require('openai')

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY })

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'public')
    },
    filename: (req, file, cb) => {
        cb(null, Date.now() + "-" + file.originalname)
    }
})

const upload = multer({ storage: storage }).single('file')
let filePath

app.post('/upload', (req, res) => {
    upload(req, res, (err) => {
        if (err instanceof multer.MulterError) {
            return res.status(500).json(err)
        }
        filePath = req.file.path
    })
})

app.post('/vision', async (req, res) => {
    try {
        const response = await openai.chat.completions.create({
            model: "gpt-4-vision-preview",
            messages: [
                {
                    role: "user",
                    content: [
                        {
                            type: "text", text: "What is in this image ?"
                        },
                        {
                            type: "image_url",
                            image_url: {
                                "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
                            },
                        },
                    ],
                },
            ],
        });
        console.log(response.choices[0]);
    } catch (error) {
        console.log(error)
    }
})


app.listen(PORT, () => console.log(`Listening on port ${PORT}`))

https://platform.openai.com/docs/guides/vision 

Visual Studio Code
Explorer
Open Editors
.env

.env
OPENAI_API_KEY= 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

Visual Studio Code
Explorer
Open Editors
server.js

server.js
// we will fill out this file during the tutorial
const PORT = 8000
const express = require('express')
const cors = require('cors')
const app = express()
app.use(cors())
app.use(express.json())
require('dotenv').config()
const fs = require('fs')
const multer = require('multer')
const OpenAI = require('openai')

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY })

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'public')
    },
    filename: (req, file, cb) => {
        cb(null, Date.now() + "-" + file.originalname)
    }
})

const upload = multer({ storage: storage }).single('file')
let filePath

app.post('/upload', (req, res) => {
    upload(req, res, (err) => {
        if (err instanceof multer.MulterError) {
            return res.status(500).json(err)
        }
        filePath = req.file.path
    })
})

app.post('/vision', async (req, res) => {
    try {
        const imageAsBase64 = fs.readFileSync(filePath, 'base64')
        const response = await openai.chat.completions.create({
            model: "gpt-4-vision-preview",
            messages: [
                {
                    role: "user",
                    content: [
                        { type: "text", text: req.body.message },
                        {
                            type: "image_url",
                            image_url: `data:image/png;base64,${imageAsBase64}`
                        },
                    ],
                },
            ],
        });
        console.log(response.choices[0]);
    } catch (error) {
        console.log(error)
    }
})


app.listen(PORT, () => console.log(`Listening on port ${PORT}`))

Visual Studio Code
Explorer
Open Editors
server.js
App.js

App.js
import { useState } from "react"

const App = () => {
  const [image, setImage] = useState(null)
  const [value, setValue] = useState("")
  const [response, setResponse] = useState("")
  const [error, setError] = useState("")

  const surpriseOptions = [
    'Does the image have a whale?',
    'Is the image fabulously pink?',
    'Does the image have puppies?'
  ]

const surprise = () => {
  const randomValue = surpriseOptions[Math.floor(Math.random() * surpriseOptions.length)]
  setValue(randomValue)
}

const uploadImage = async (e) => {
  setResponse("")
  const formData = new formData()
  formData.append('file', e.target.files[0])
  setImage(e.target.files[0])
  e.target.value = null
  try {
    const options = {
      method: 'POST',
      body: formData
    }
    const response = await fetch('http://localhost:8000/upload', options)
    const data = response.json()
    console.log(data)
  }
  catch (error) {
    console.error(error)
    setError("Something did not work ! Please try again")
  }
}

const analyzeImage = async () => {
  setResponse("")
  if (!image) {
    setError("Error ! Must have an existing image !")
    return
  }
  try {
    const options = {
      method: 'POST',
      body: JSON.stringify({
        message: value
      }),
      headers: {
        'Content-Type': 'application/json'
      }
    }
    const response = await fetch('http://localhost:8000/vision', options)
    const data = response.json()
    console.log(data)
  } catch (error) {
    console.log(error)
    setError("Something did not work. Please try again.")
  }
}

const clear = () => {
  setImage(null)
  setValue("")
  setResponse("")
  setError("")
}

  return (
    <div className="app">
      <section className="search-section">
        <div className="image-container">
          {image && <img className="image" src={URL.createObjectURL(image)} />}
        </div>
        <p className="extra-info">
          <span>
            <label htmlFor="files" className="upload"> upload an image </label>
            <input onChange={uploadImage} id="files" accept="image/*" type="file" hidden />
          </span>
          to ask questions about.
        </p>
        <p>What do you want to know about the image?
          <button className="surprise" onClick={"surprise"} disabled={response}>Surprise me</button>
        </p>
        <div className="input-container">
          <input
            value={value}
            placeholder="What is in the image..."
            onChange={e => setValue(e.target.value)}
          />
          {(!response && !error) && <button onClick={analyzeImage}>Ask me</button>}
          {(response || error) && <button onClick={clear}>Clear</button>}
        </div>
        {error && <p>{""}</p>}
        {response && <p className="answer">{""}</p>}
      </section>
    </div>
  )
}

export default App

Visual Studio Code
Explorer
Open Editors
server.js

server.js
// we will fill out this file during the tutorial
const PORT = 8000
const express = require('express')
const cors = require('cors')
const app = express()
app.use(cors())
app.use(express.json())
require('dotenv').config()
const fs = require('fs')
const multer = require('multer')
const OpenAI = require('openai')

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY })

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'public')
    },
    filename: (req, file, cb) => {
        cb(null, Date.now() + "-" + file.originalname)
    }
})

const upload = multer({ storage: storage }).single('file')
let filePath

app.post('/upload', (req, res) => {
    upload(req, res, (err) => {
        if (err instanceof multer.MulterError) {
            return res.status(500).json(err)
        }
        filePath = req.file.path
    })
})

app.post('/vision', async (req, res) => {
    try {
        const imageAsBase64 = fs.readFileSync(filePath, 'base64')
        const response = await openai.chat.completions.create({
            model: "gpt-4-vision-preview",
            messages: [
                {
                    role: "user",
                    content: [
                        { type: "text", text: req.body.message },
                        {
                            type: "image_url",
                            image_url: `data:image/png;base64,${imageAsBase64}`
                        },
                    ],
                },
            ],
        });
        console.log(response.choices[0]);
        res.send(response.choices[0])
    } catch (error) {
        console.log(error)
    }
})


app.listen(PORT, () => console.log(`Listening on port ${PORT}`))

Visual Studio Code
Explorer
Open Editors
server.js
App.js

App.js
import { useState } from "react"

const App = () => {
  const [image, setImage] = useState(null)
  const [value, setValue] = useState("")
  const [response, setResponse] = useState("")
  const [error, setError] = useState("")

  const surpriseOptions = [
    'Does the image have a whale?',
    'Is the image fabulously pink?',
    'Does the image have puppies?'
  ]

const surprise = () => {
  const randomValue = surpriseOptions[Math.floor(Math.random() * surpriseOptions.length)]
  setValue(randomValue)
}

const uploadImage = async (e) => {
  setResponse("")
  const formData = new formData()
  formData.append('file', e.target.files[0])
  setImage(e.target.files[0])
  e.target.value = null
  try {
    const options = {
      method: 'POST',
      body: formData
    }
    const response = await fetch('http://localhost:8000/upload', options)
    const data = response.json()
    console.log(data)
  }
  catch (error) {
    console.error(error)
    setError("Something did not work ! Please try again")
  }
}

const analyzeImage = async () => {
  setResponse("")
  if (!image) {
    setError("Error ! Must have an existing image !")
    return
  }
  try {
    const options = {
      method: 'POST',
      body: JSON.stringify({
        message: value
      }),
      headers: {
        'Content-Type': 'application/json'
      }
    }
    const response = await fetch('http://localhost:8000/vision', options)
    const data = await response.json()
    console.log(data)
    setResponse(data.message.content)
  } catch (error) {
    console.log(error)
    setError("Something did not work. Please try again.")
  }
}

const clear = () => {
  setImage(null)
  setValue("")
  setResponse("")
  setError("")
}

  return (
    <div className="app">
      <section className="search-section">
        <div className="image-container">
          {image && <img className="image" src={URL.createObjectURL(image)} />}
        </div>
        <p className="extra-info">
          <span>
            <label htmlFor="files" className="upload"> upload an image </label>
            <input onChange={uploadImage} id="files" accept="image/*" type="file" hidden />
          </span>
          to ask questions about.
        </p>
        <p>What do you want to know about the image?
          <button className="surprise" onClick={"surprise"} disabled={response}>Surprise me</button>
        </p>
        <div className="input-container">
          <input
            value={value}
            placeholder="What is in the image..."
            onChange={e => setValue(e.target.value)}
          />
          {(!response && !error) && <button onClick={analyzeImage}>Ask me</button>}
          {(response || error) && <button onClick={clear}>Clear</button>}
        </div>
        {error && <p>{""}</p>}
        {response && <p className="answer">{response}</p>}
      </section>
    </div>
  )
}

export default App





