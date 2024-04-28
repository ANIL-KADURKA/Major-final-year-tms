<!-- <template>
  <div class="home-div">
    <h1 style="text-align: center">
      <span>Welcome to</span><br /><span>Online Support Ticket System</span>
    </h1>
    <b-row align-h="end">
      <b-button
        style="margin: 10px; margin-right: 50px"
        type="button"
        variant="primary"
        :to="{ path: '/login' }"
        >Login</b-button
      ></b-row
    >
  </div>
</template>

<script>
import * as common from "../assets/common.js";

export default {
  name: "AppHomeView",
  components: {},
  data() {
    return {
      show: true,
    };
  },
  methods: {},
};
</script>

<style>
.home-div {
  background-image: url("../assets/home_page_image.jpg");
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style> -->


<template>

  <div>

    <nav class="navbar navbar-expand-lg fixed-top ">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarText">
          <ul class="navbar-nav m-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <b-link class="nav-link route-element" href="/login">Login</b-link>
            </li>
            <li class="nav-item">
              <b-link class="nav-link route-element" href="/register">Signup</b-link>
            </li>
          </ul>
        </div>

      </div>
    </nav>

    <div class="keyart" id="parallax">
      <div class="keyart_layer parallax" id="keyart-0" data-speed="4"></div>
      <div class="keyart_layer parallax" id="keyart-1" data-speed="8"></div>
      <div class="keyart_layer parallax" id="keyart-2" data-speed="15"></div>
      <div class="keyart_layer parallax" id="keyart-3" data-speed="20"></div>
      <div class="keyart_layer parallax" id="keyart-4" data-speed="30">
        <h1 class="text-center title ">Welcome to Online Ticket Management System</h1>
      </div>
      <div class="keyart_layer parallax" id="keyart-5" data-speed="42"></div>
      <div class="keyart_layer parallax" id="keyart-6" data-speed="55"></div>
      <div class="keyart_layer" id="keyart-scrim"></div>
      <div class="keyart_layer parallax" id="keyart-7" data-speed="76"></div>
      <div class="keyart_layer" id="keyart-8" data-speed="110"></div>
    </div>

  </div>

</template>

<style>
body,
#parallax {
  margin: 0;
  padding: 0;
}

.title {
  margin-top: 120px;
  position: relative;
  font-family: 'Roboto';
  font-weight: 800;
}

.keyart,
.keyart_layer {
  height: 1050px;

}

.route-element {
  font-size: 22px;
  font-family: 'Roboto';
  cursor: pointer;
  color:white;
  margin-right:20px; 
}

#parallax {
  display: block;
}

.keyart {
  position: relative;
  z-index: 10;
}

.keyart_layer {
  background-position: bottom center;
  background-size: auto 1038px;
  background-repeat: repeat-x;
  width: 100%;
  position: absolute;
}

.keyart_layer.parallax {
  position: fixed;
}

#keyart-0 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax0.png');
  background-color: #ffaf1b;
}

#keyart-1 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax1.png');
}

#keyart-2 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax2.png');
}

#keyart-3 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax3.png');
}

#keyart-4 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax4.png');
}

#keyart-5 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax5.png');
}

#keyart-6 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax6.png');
}

#keyart-7 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax7.png');
}

#keyart-8 {
  background-image: url('http://www.firewatchgame.com/images/parallax/parallax8.png');
}

#keyart-scrim {
  background-color: #ffaf00;
  opacity: 0;
}

@media (-webkit-min-device-pixel-ratio: 1.5),
(min-resolution: 72dpi) {
  #keyart-0 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax0@2x.png');
    background-color: #ffaf1b;
  }

  #keyart-1 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax1@2x.png');
  }

  #keyart-2 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax2@2x.png');
  }

  #keyart-3 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax3@2x.png');
  }

  #keyart-4 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax4@2x.png');
  }

  #keyart-5 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax5@2x.png');
  }

  #keyart-6 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax6@2x.png');
  }

  #keyart-7 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax7@2x.png');
  }

  #keyart-8 {
    background-image: url('http://www.firewatchgame.com/images/parallax/parallax8@2x.png');
  }
}
</style>

<script>
export default {
  name: "AppHomeView",
  components: {},
  data() {
    return {
      show: true,
    };
  },
  mounted() {
    this.castParallax();
  },
  methods: {
    castParallax() {
      window.addEventListener("scroll", () => {
        let top = window.pageYOffset;
        let layers = document.getElementsByClassName("parallax");
        for (let i = 0; i < layers.length; i++) {
          let layer = layers[i];
          let speed = layer.getAttribute('data-speed');
          let yPos = -(top * speed / 100);
          layer.style.transform = 'translate3d(0px, ' + yPos + 'px, 0px)';
        }
      });
    }
  }
}
</script>
