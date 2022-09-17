<style>
.wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 30px;    
}

.sipher {
    position: absolute;
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
    border-radius: 30px;
    padding: 12px;
    align-items: right;
    height: 1%;
    animation: fadeIn 50s;
    -webkit-animation: fadeIn 5s;
    -moz-animation: fadeIn 5s;
    -o-animation: fadeIn 5s;
    -ms-animation: fadeIn 5s;
}
@keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
  
  @-moz-keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
  
  @-webkit-keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
  
  @-o-keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
  
  @-ms-keyframes fadeIn {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
.card-title {
    color: #fff;
    font-size: 12px;
    font-weight: thin;
    margin-bottom: 16px;
    font-family: monospace;
    opacity: .5
}

.user-profile {
    display: flex;
    gap: 1em
}

.Profile-pic {
    position: relative
}

#pfp {
    width: 128px;
    height: 128px;
    border-radius: 10%;
    -webkit-transition-property: all; 
    -webkit-transition-duration: 0.3s; 
    -webkit-transition-timing-function: ease; 
}
#pfp:hover { 
    transform: scale(1.1); 

}
.connections {
    position:absolute;
    display: flex;
    align-items: center;
    width: 100%;
    gap: 10px;
    margin-top: 5px
}

.connection-icon {
    height: 28px
}
.connection-icon:hover { 
    transform: scale(1.5); 
    transform: rotate(360deg);
    transition: all 0.5s ease-in-out 0s;
}

#activity-dot {
    border-radius: 50%;
    width: 20px;
    height: 20px;
    background-color: #fff;
    position: absolute;
    top: 0;
    right: 0;
    transform: translate(50%, -50%);
    border: 2px solid rgba(0,0,0, .3)
}

#username {
    font-size: 28px;
    font-weight: 700
}
#status2, #status {
    opacity: .5;
    transition: all .6s ease;
}
@media (max-width : 768px ){
    .card, .card1 {
        backdrop-filter: blur(3px);
        -webkit-backdrop-filter: blur(5px);
        border-radius: 30px;
        padding: 12px;
        align-items: right;
        height: 1%
    }
    .card-title {
        color: #fff;
        font-size: 12px;
        font-weight: thin;
        margin-bottom: 16px;
        font-family: monospace;
        opacity: .5
    }
    #username {
        font-size: 20px;
        font-weight: 700
    }
    #status2, #status {
        opacity: .5;
        transition: all .6s ease;
        font-size: 14px;
    }
}

.spotify-outerbar {
    margin-top: 5px;
    width: 100%;
    height: 6px;
    border-radius: 150px;
    background-color: #1C1D22
}

#spotify-innerbar {
    height: 6px;
    border-radius: 150px;
    background-color: #fff
}

.spotify-time {
    margin-top: 2px;
    width: 100%;
    display: flex;
    justify-content: space-between
}
</style>

<script>
API_URL = `https://api.lanyard.rest/v1`;
//USERID = `160279076727160832`;
userIDS = ["932635118818979991","160279076727160832"]
// document.addEventListener(`contextmenu`, (e) => e.preventDefault());
// function ctrlShiftKey(e, keyCode) {
//   return e.ctrlKey && e.shiftKey && e.keyCode === keyCode.charCodeAt(0);
// }
// window.onscroll = function () {
//     window.scrollTo(0,0);
// }
// document.onkeydown = (e) => {
//   // Disable F12, Ctrl + Shift + I, Ctrl + Shift + J, Ctrl + U
//   if (
//     event.keyCode === 123 ||
//     ctrlShiftKey(e, `I`) ||
//     ctrlShiftKey(e, `J`) ||
//     ctrlShiftKey(e, `C`) ||
//     (e.ctrlKey && e.keyCode === `U`.charCodeAt(0))
//   )
//     return false;
// };
async function fetchResponse(userId) {
    try {
        const url = await fetch(`${API_URL}/users/${userId}`);
        const response = await url.json();
        return response;
    } catch (error) {
        console.error(error);
    }
}

async function setAvatar(USERID, card) {
    const response = await fetchResponse(USERID);
    var avatarId = response.data.discord_user.avatar;
    var fullUrl = `https://cdn.discordapp.com/avatars/${USERID}/${avatarId}`;
    if (response.data.discord_user.avatar) document.getElementsByClassName(`${card}-pfp`)[0].src = fullUrl;
    else document.getElementsByClassName(`${card}-pfp`)[0].src = "https://cdn.discordapp.com/embed/avatars/1.png"
}

async function setAvatarFrame(USERID, card) {
    const response = await fetchResponse(USERID);
    const activity2 = document.getElementsByClassName(`${card}-status2`)[0];
    switch (response.data.discord_status) {
        case `online`:
            document.getElementsByClassName(`${card}-activity-dot`)[0].style.background = `#3ba45d`;
            document.getElementsByClassName(`${card}-activity-dot`)[0].title = `Online`;
            activity2.innerHTML = `Online`;
            activity2.style.cssText = `color: #3ba45d; opacity: 0.5;`;
            break;
        case `dnd`:
            document.getElementsByClassName(`${card}-activity-dot`)[0].style.background = `#ed4245`;
            document.getElementsByClassName(`${card}-activity-dot`)[0].title = `Do not disturb`;
            activity2.innerHTML = `Do not disturb`;
            activity2.style.cssText = `color: #ed4245; opacity: 0.5;`;
            break;
        case `idle`:
            document.getElementsByClassName(`${card}-activity-dot`)[0].style.background = `#faa81a`;
            document.getElementsByClassName(`${card}-activity-dot`)[0].title = `Idle`;
            activity2.innerHTML = `Idle`;
            activity2.style.cssText = `color: #faa81a; opacity: 0.5;`;
            break;
        case `offline`:
            document.getElementsByClassName(`${card}-activity-dot`)[0].style.background = `#747e8c`;
            document.getElementsByClassName(`${card}-activity-dot`)[0].title = `Offline`;
            activity2.innerHTML = `Offline`;
            activity2.style.cssText = `color: #747e8c; opacity: 0.5;`;
            break;
    }
    let presence = response.data.activities.find(r=> r.type === 1)
    if (presence) {
        if (presence.url.includes("twitch.tv")) {
            document.getElementsByClassName(`${card}-activity-dot`)[0].style.background = `#301934`;
            document.getElementsByClassName(`${card}-activity-dot`)[0].title = `Streaming`;
            activity2.innerHTML = `Streaming`;
            activity2.style.cssText = `color: #301934; opacity: 0.5;`;
        }
    }

}
async function setUsername(USERID, card) {
    const response = await fetchResponse(USERID);
    var user = response.data.discord_user.username;
    var discriminator = response.data.discord_user.discriminator;
    var fullName = `${user}#${discriminator}`;
    document.getElementsByClassName(`${card}-username`)[0].innerHTML = fullName;
    document.getElementById(`${card}-title`).innerHTML = `Owner - @${user}`;
    document.getElementById(`${card}-connection-username`).title = fullName;
}
async function onHover(USERID, card) {
    const response = await fetchResponse(USERID);
    let rpc = response.data.activities.find(r=> r.type === 0)
    console.log(rpc)
    // PFP //
    if(response.data.listening_to_spotify === true) document.getElementsByClassName(`${card}-pfp`)[0].src = response.data.spotify.album_art_url 
    else if(rpc) document.getElementsByClassName(`${card}-pfp`)[0].src = `https://cdn.discordapp.com/app-assets/` + rpc.application_id + `/` + rpc.assets.large_image + `.png` 

    // USERNAME //
    if (response.data.listening_to_spotify === true) document.getElementsByClassName(`${card}-username`)[0].innerHTML = "Listening to Spotify"
    else if (rpc) document.getElementsByClassName(`${card}-username`)[0].innerHTML = rpc.name
    
    // TITLE //
    if(response.data.listening_to_spotify === true) document.getElementById(`${card}-title`).innerHTML = `Owner - @${response.data.discord_user.username} > Playing "${response.data.spotify.song}"`
    else if (rpc) document.getElementById(`${card}-title`).innerHTML = `Owner - @${response.data.discord_user.username} > Playing ${rpc.name}`
    
    //ARTIST //
    if(response.data.listening_to_spotify === true) document.getElementsByClassName(`${card}-status`)[0].innerHTML = response.data.spotify.artist
    else if(rpc) document.getElementsByClassName(`${card}-status`)[0].innerHTML = rpc.details
    
    // if(undefined) {} 
    else {document.getElementsByClassName(`${card}-status`)[0].innerHTML = response.data.spotify.artist}
}

async function offHover(USERID, card) {
    const response = await fetchResponse(USERID);
    document.getElementsByClassName(`${card}-username`)[0].innerHTML = `${response.data.discord_user.username}#${response.data.discord_user.discriminator}`
    if (response.data.discord_user.avatar) document.getElementsByClassName(`${card}-pfp`)[0].src = `https://cdn.discordapp.com/avatars/${USERID}/${response.data.discord_user.avatar}` 
    else document.getElementsByClassName(`${card}-pfp`)[0].src = "https://cdn.discordapp.com/embed/avatars/1.png"
    document.getElementById(`${card}-title`).innerHTML = `Owner - @${response.data.discord_user.username}`;
    let status = response.data.activities[0].name;
    if(response.data.activities.find(r=> r.type === 4)) {
        let customstat =response.data.activities.find(r=> r.type === 0)
        document.getElementsByClassName(`${card}-status`)[0].innerHTML = `${customstat.state}`
    }
    else if (response.data.listening_to_spotify === true ) {
        if (document.getElementById(`${card}-title`).innerHTML === `Owner - @${response.data.discord_user.username} > Listening to Spotify`) return;
        return document.getElementsByClassName(`${card}-status`)[0].innerHTML = "Listening to Spotify"
    } else {
        let rpc1 = response.data.activities.find(r=> r.type === 3)
        if (rpc1)  return document.getElementsByClassName(`${card}-status`)[0].innerHTML = `Watching ${rpc1.name}`
        let rpc2 = response.data.activities.find(r=> r.type === 1)
        if (rpc2)  return document.getElementsByClassName(`${card}-status`)[0].innerHTML = `Streaming ${rpc2.name}`
        let rpc3 = response.data.activities.find(r=> r.type === 5)
        if (rpc3)  return document.getElementsByClassName(`${card}-status`)[0].innerHTML = `Competing in ${rpc3.name}`
        if(status === undefined) return document.getElementsByClassName(`${card}-status`)[0].innerHTML = `Status unavailable.`;
        document.getElementsByClassName(`${card}-status`).innerHTML = `Status unavailable.`;
    }
}
async function setStatus(USERID, card) {
    const response = await fetchResponse(USERID);
    let status = response.data.activities[0].name;
    if (response.data.discord_status == `offline`) {
        return;
    }
    if (response.data.listening_to_spotify === true) {
        return document.getElementsByClassName(`${card}-status`).innerHTML = "Listening to Spotify"
    }
    let rpc1 = response.data.activities.find(r=> r.type === 3)
    if (rpc1)  return document.getElementsByClassName(`${card}-status`).innerHTML = `Watching ${rpc1.name}`
    let rpc2 = response.data.activities.find(r=> r.type === 1)
    if (rpc2)  return document.getElementsByClassName(`${card}-status`).innerHTML = `Streaming ${rpc2.name}`
    let rpc3 = response.data.activities.find(r=> r.type === 5)
    if (rpc3)  return document.getElementsByClassName(`${card}-status`).innerHTML = `Competing in ${rpc3.name}`
    if(status === undefined) return document.getElementById(`${card}-status`).innerHTML = `Status unavailable.`;
    document.getElementById(`${card}-status`).innerHTML = `Status unavailable.`;
}
async function setSpotifyBar(USERID, card) {
    const response = await fetchResponse(USERID);
    var bar = document.getElementsByClassName(`${card}-spotify-innerbar`)[0];
    var bar2 = document.getElementsByClassName(`${card}-spotify-time-end`)[0];
    var bar3 = document.getElementsByClassName(`${card}-spotify-time-start`)[0];
    if (response.data.listening_to_spotify == false) {
        bar.style.display = `none`;
        bar2.innerHTML = ``;
        bar3.innerHTML = ``;
        return;
    }
    const date = new Date().getTime();
    const v1 = response.data.spotify.timestamps.end -
        response.data.spotify.timestamps.start;
    const v2 = date - response.data.spotify.timestamps.start;

    function spotifyTimeSet(date, element) {
        const x = document.getElementsByClassName(element)[0];
        const y = new Date(date);
        const minutes = y.getMinutes();
        const seconds = y.getSeconds();
        const formmatedseconds = seconds < 10 ? `0${seconds}` : seconds;
        x.innerHTML = `${minutes}:${formmatedseconds}`;
    }
    spotifyTimeSet(v1, `${card}-spotify-time-end`);
    spotifyTimeSet(v2, `${card}-spotify-time-start`);
    prcnt = (v2 / v1) * 100;
    precentage = Math.trunc(prcnt);
    prccc = Math.round((prcnt + Number.EPSILON) * 100) / 100;
    i = 1;
    bar.style.display = `block`;
    bar.style.width = prccc + `%`;
}

async function setRichPresenceName(USERID, card) {
    const response = await fetchResponse(USERID);
    var par = document.getElementById(`${card}-richpresence-song`);
    let rpc = response.data.activities.find(r=> r.type === 0)
    if (!rpc) {
        let rpc1 = response.data.activities.find(r=> r.type === 3)
        if (rpc1) return par.innerHTML = `Watching ${rpc1.name}`
        let rpc2 = response.data.activities.find(r=> r.type === 1)
        if (rpc2) return par.innerHTML = `Streaming ${rpc2.name}`
        let rpc3 = response.data.activities.find(r=> r.type === 5)
        if (rpc3) return par.innerHTML = `Competing ${rpc3.name}`
        par.innerHTML ="There are no games being played.";
        return;
    }
    par.style.display = `block`;
    par.innerHTML = rpc.name;
}
async function setRichPresenceCover(USERID, card) {
    const response = await fetchResponse(USERID);
    var par = document.getElementById(`${card}-rpc-cover`);
    let rpc = response.data.activities.find(r=> r.type === 0)
    if (!rpc) {
        par.style.display = `none`;
        return;
    }
    let cover = `https://cdn.discordapp.com/app-assets/` + rpc.application_id + `/` + rpc.assets.large_image + `.png`
    par.style.display = `block`;
    par.src = cover;
}
async function setRichPresenceArtist(USERID, card) {
    const response = await fetchResponse(USERID);
    var par = document.getElementById(`${card}-richpresence-artist`);
    let rpc = response.data.activities.find(r=> r.type === 0)
    if (!rpc) {
        par.style.display = `none`;
        return;
    }
    par.style.display = `block`;
    par.innerHTML = rpc.details;
}

async function setRichPresenceAlbum(USERID, card) {
    const response = await fetchResponse(USERID);
    var par = document.getElementById(`${card}-richpresence-album`);
    let rpc = response.data.activities.find(r=> r.type === 0)
    if (!rpc) {
        par.style.display = `none`;
        return;
    }
    par.style.display = `block`;
   // par.innerHTML = rpc.details;
}

async function presenceInvoke(USERID, card) {
    setSpotifyAlbumCover(USERID, card);
    setSpotifyArtist(USERID, card);
    setSpotifySongName(USERID, card);
    setSpotifyBar(USERID, card);
}

async function richpresenceInvoke(USERID, card) {
    setRichPresenceCover(USERID, card);
    setRichPresenceArtist(USERID, card);
    setRichPresenceName(USERID, card);
    setRichPresenceAlbum(USERID, card);
}

async function statusInvoke(USERID, card) {
    setStatus(USERID, card);
    setAvatarFrame(USERID, card);
}
async function invoke(USERID, card) {

    setAvatar(USERID, card);
    setUsername(USERID, card);
    setInterval(function() {
        setSpotifyBar(USERID, card);
        statusInvoke(USERID, card)
    }, 1000)
    let rape = document.getElementById("sipher")
    console.log(rape)
    
    rape.hidden=false

    // setInterval(richpresenceInvoke(USERID, card), 1000);
}
// userIDS.forEach(user => {
//     let wrapper = document.getElementsByClassName("wrapper")
//     wrapper.innerHTML += (`
//     <div class="card">
//         <div id="card-name" style="color: #fff;" class="card-title"></div>
//         <div style="color: #fff;" class="user-profile">
//             <div class="Profile-pic">
//                 <img id="pfp" src="domain/assets/assets/loader.gif" alt="">
//                 <div style="color: #fff;" id="activity-dot" aria-label="Unknown"></div>
//                 <div style="display: block; color: #fff;" id="username">Loading...</div>
//                 <div style="color: #fff;" id="status"></div>
//                 <div style="color: #fff;" id="status2">unknown</div>
//             </div>
//             <div class="user-info">
//                 <div class="connections">
//                     <a title="siph.er" href="https://instagram.com/siph.er"><img class="connection-icon" src="domain/assets/assets/logo/ig.svg" alt=""></a>
//                     <a id="connection-username" title="" href="https://discord.com/users/160279076727160832/"><img class="connection-icon" src="domain/assets/assets/connections/discord.svg" alt=""></a>
//                     <a title="sipher3" href="https://github.com/sipher3/"><img class="connection-icon" src="domain/assets/assets/connections/github.svg" alt=""></a>
//                 </div>
//             </div>
//         </div>
//     </div>`)
//     invoke(user);
//     console.log(`done ${user}`)
// })
</script>

<div class="wrapper">
    <div id="sipher" hidden="true">
        <div id="sipher-title" style="color: #fff;" class="card-title"></div>
        <div id="sipher-user-profile"style="color: #fff;" class="user-profile">
            <div class="Profile-pic">
                <img onmouseover="onHover(`160279076727160832`,`sipher`);" onmouseout="offHover(`160279076727160832`,`sipher`);" class="sipher-pfp" id="pfp" src="assets/loader.gif" alt="">
                <div class="sipher-activity-dot" style="color: #fff;" id="activity-dot" aria-label="Unknown"></div>
            </div>
            <div class="user-info">
                <div class="sipher-username" style="color: #fff;" id="username">Loading...</div>
                <div class="sipher-status" style="color: #fff;" id="status"></div>
                <div class="sipher-status2" style="color: #fff;" id="status2">unknown</div>
                <div class="spotify-outerbar">
                    <div class="sipher-spotify-innerbar" id="spotify-innerbar"></div>
                </div>
                <div class="spotify-time">
                    <div class="sipher-spotify-time-start" style="color: #fff;" id="spotify-time-start"></div>
                    <div class="sipher-spotify-time-end" style="color: #fff;" id="spotify-time-end"></div>
                </div>
                <div class="connections">
                    <a title="fsipher" href="https://instagram.com/fsipher"><img class="connection-icon" src="assets/connections/ig.svg" alt=""></a>
                    <a id="sipher-connection-username" title="" href="https://discord.com/users/160279076727160832/"><img class="connection-icon" src="assets/connections/discord.svg" alt=""></a>
                    <a title="sipher3" href="https://github.com/sipher3/"><img class="connection-icon" src="assets/connections/github.svg" alt=""></a>
                    <a title="siph3r" href="https://t.me/siph3r"><img class="connection-icon" src="assets/connections/telegram.svg" alt=""></a>
                    <a title="siph4r" href="https://twitter.com/siph4r"><img class="connection-icon" src="assets/connections/twitter.svg" alt=""></a>
                    <a title="@knee" href="https://tiktok.com/@knee"><img class="connection-icon" src="assets/connections/tiktok.svg" alt=""></a>
                </div>
            </div>
        </div>
    </div>
    <script>invoke(`160279076727160832`,`sipher`)</script>
</div>
