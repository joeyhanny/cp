<template>
  <div class="about">
    <h1>About Us</h1>
    <img class="about-page-img" src="/images/couch-potatoes-3119968_1920.jpg" alt="">
    <p class="about-bio">Ever since we started dating, we have always enjoyed cooking together: simple classics, restaurant copycats, complicated cuisines from cultures around the world. Our only limits were our college budgets and our Wi-Fi connection speeds.</p>

    <p class="about-bio">Since most of our recipes came from internet searches, we soon faced a rather taxing problem. Most recipe websites are not just recipe repositories. They are blogs, journals, social media pages, tell-your-whole-life-to-the-internet-outlets, and anything in between. They are also the home of never-ending pop-up since they are often funded by advertisers.</p>

    <p class="about-bio">At first, we used the whole “pretend to print the page but actually just download as a screenshot/pdf” trick, but that led to the difficulty of finding those files again – which often resulted in visiting the website again after our initial searches failed. This trick also does not help us when we want to add notes about substitutions, add-ons, or other recipe alterations we have found satisfying.</p>

    <p class="about-bio">Thus, Couch and Potato was born as a storage for our beloved recipes. It was originally intended just for family, but since it is on the internet some of you reading this might be total strangers. Either way, we invite you to enjoy our little collection. The website is not commercial, so you will never find a single ad stopping you from getting to the point – which is, of course, the food.</p>

    <p class="about-bio">A quick note about sources. Some of you may quite enjoy reading the blogs that accompany these recipes, and if so, we would like to empower you to do so. We also wanted to give credit where it is due, since we did not invent these recipes ourselves. You will find a link to the source website at the bottom of each recipe. Despite copyright law not covering recipe ingredient lists or instructions, we want to acknowledge the creativity, and oftentimes genius, of the creators of these recipes. To each of them, we owe a humble thank you!</p>
  </div>
</template>

<style>
  .about {
    margin-left: 45px;
    margin-right: 45px;
  }

  .about-page-img {
    float: left;
    width: 50%;
    height: auto;
    margin-right: 20px;
    margin-top: 10px;
  }

  .about-bio {
    text-align: left;
    line-height: 1.5;
    margin-bottom: 15px;
  }

  /* Tablet Styles */
  @media only screen and (min-width: 501px) and (max-width: 960px) {
    .about-page-img {
      width: 40%;
    }

    .about-bio {
      font-size: 20px;
    }

  }

  /* Small Desktop Styles */
  @media only screen and (min-width: 961px) {
    .about-page-img {
      width: 30%;
    }

    .about-bio {
      font-size: 20px;
    }


  }
</style>
