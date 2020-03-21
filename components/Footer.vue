<template>
  <div id="contact" class="footer-area">
    <div class="footer-left">
      <p>
        © 2020 <a target="_blank" href="">Mohin</a> Inc. All rights reserved.
      </p>
    </div>
    <div class="footer-left">
      <div class="social-icon">
        <ul>
          <li>
            <a href="https://www.facebook.com/mohincse/" target="_blank"
              ><i class="fa fa-facebook" aria-hidden="true"></i
            ></a>
          </li>
          <li>
            <a href="https://twitter.com/mohincse" target="_blank"
              ><i class="fa fa-twitter" aria-hidden="true"></i
            ></a>
          </li>
          <li>
            <a href="https://www.instagram.com/mohincse/" target="_blank"
              ><i class="fa fa-instagram" aria-hidden="true"></i
            ></a>
          </li>
          <li>
            <a href="https://github.com/csemohin" target="_blank"
              ><i class="fa fa-github" aria-hidden="true"></i
            ></a>
          </li>
          <li>
            <a href="https://www.freelancer.com/u/mohincse" target="_blank"
              ><i class="fa fa-globe" aria-hidden="true"></i
            ></a>
          </li>
          <li>
            <a
              href="https://www.linkedin.com/in/mohin-uddin-a1152614b/"
              target="_blank"
              ><i class="fa fa-linkedin" aria-hidden="true"></i
            ></a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.footer-area {
  margin-top: 30px;
  border-radius: 10px;
  background: #eaf2f9;
  box-shadow: 6px 6px 7px #e5edf4, -6px -6px 7px #eff7fe;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 30px 20px;
}

.footer-left p {
  font-size: 16px;
  margin: 0;
  line-height: 1;
  opacity: 0.5;
}
.footer-left a {
  color: #000;
  font-weight: 500;
}
.social-icon ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.social-icon ul li {
  display: inline-block;
}

.social-icon ul li a {
  display: block;
  color: #333;
  width: 30px;
  height: 30px;
  margin-left: 10px;
  text-align: center;
  line-height: 30px;
  border-radius: 5px;
  background: #eaf2f9;
  box-shadow: 5px 5px 10px #e1e8ef, -5px -5px 10px #f3fcff;
  transition: 0.3s ease-in-out;
}
.social-icon ul li a:hover {
  background: #eaf2f9;
  box-shadow: inset -5px -5px 10px #e1e8ef, inset 5px 5px 10px #f3fcff;
}
</style>
