
<script setup>
  import uitoolkit from "@zoom/videosdk-ui-toolkit";
  import '@zoom/videosdk-ui-toolkit/dist/videosdk-ui-toolkit.css'

  var sessionContainer
  var authEndpoint = ''
  var config = {
      videoSDKJWT: '',
      sessionName: 'test',
      userName: 'Vue.js',
      sessionPasscode: '123',
      featuresOptions: {
        virtualBackground: {
            enable: true,
            virtualBackgrounds: [
                {
                    url: 'https://images.unsplash.com/photo-1715490187538-30a365fa05bd?q=80&w=1945&auto=format&fit=crop'
                },
            ],
        },
    },
  };
  var role = 1

  function getVideoSDKJWT() {
    sessionContainer = document.getElementById('sessionContainer')

    document.getElementById('join-flow').style.display = 'none'

    fetch(authEndpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
          sessionName:  config.sessionName,
          role: role,
      })
    }).then((response) => {
        return response.json()
    }).then((data) => {
      if(data.signature) {
        console.log(data.signature)
        config.videoSDKJWT = data.signature
        joinSession()
      } else {
        console.log(data)
      }
    }).catch((error) => {
        console.log(error)
    })
  }

  function joinSession() {
    uitoolkit.joinSession(sessionContainer, config)

    uitoolkit.onSessionClosed(sessionClosed)
  }

  var sessionClosed = (() => {
    console.log('session closed')
    uitoolkit.closeSession(sessionContainer)

    document.getElementById('join-flow').style.display = 'block'
  })
</script>

<template>
  <main>
    <div id="join-flow">
      <h1>Zoom Video SDK Sample Vue.js</h1>
      <p>User interface offered by the Video SDK UI Toolkit</p>

      <button @click="getVideoSDKJWT">Join Session</button>
    </div>

    <div id='sessionContainer'></div>
  </main>
</template>

<style scoped>

</style>
