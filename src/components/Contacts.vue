<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue';
import { Notyf } from 'notyf';
import 'notyf/notyf.min.css';

const notyf = new Notyf();
const WEB3FORMS_ACCESS_KEY = "39652787-7b33-41f8-8e54-1470783ce322";

const subject = "New message from Portfolia Contact Form";
const name = ref("");
const email = ref("");
const message = ref("");
const isLoading = ref(false);

const SITE_KEY = "6LfYENwrAAAAADWHachZhYGQu4Q_4vof5Z50vthV";
const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref("");

function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

function onRecaptchaExpired() {
  recaptchaToken.value = "";
}

function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error("reCAPTCHA not loaded");
    return
  }

  recaptchaWidgetId.value = window.grecaptcha.render(
    recaptchaContainer.value, {
      sitekey: SITE_KEY,
      size: "normal",
      callback: onRecaptchaSuccess,
      "expired-callback": onRecaptchaExpired,
    }
  )
}

function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = "";
  }
}

const submitForm = async () => {
  if (!recaptchaToken.value) {
    notyf.error("Please verify that you are not a robot.");
    return;
  }

  isLoading.value = true;
  try {
    const response = await fetch(
      "https://api.web3forms.com/submit", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          access_key: WEB3FORMS_ACCESS_KEY,
          subject: subject,
          name: name.value,
          email: email.value,
          message: message.value,
        }),
      }
    );
    const result = await response.json();

    if (result.success) {
      console.log(result);
      isLoading.value = false;
      notyf.success("Message sent!");
    }
  } catch (error) {
    console.log(error);
    isLoading.value = false;
    notyf.error("Failed to send message");
  } finally {
    resetRecaptcha();
  }
}

onMounted(() => {
  const interval = setInterval(() => {
    if (window.grecaptcha && window.grecaptcha.render){
      renderRecaptcha();
      clearInterval(interval);
    }
  }, 100);

  onBeforeUnmount(() => {
    clearInterval(interval);
  })

})

</script>

<template>
  <section id="contact">
    <h2 class="text-center">Contact</h2>
    <div class="container my-5">
      <div class="row">
        <div class="col-lg-6 my-5 my-lg-0">
          <iframe
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d6120.373230164688!2d116.39631257233809!3d39.91483981780522!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x35f052e94515d43d%3A0x674e2bd4dd3079f!2sForbidden%20City!5e0!3m2!1sen!2sph!4v1751459910464!5m2!1sen!2sph"
            id="map" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
        </div>

        <div class="col-lg-6 h-100">
          <form @submit.prevent="submitForm">
            <div class="mb-3">
              <input type="text" v-model="name" class="form-control" id="fullname" placeholder="First Name M.I. Last Name">
            </div>
            <div class="mb-3">
              <input type="email" v-model="email" class="form-control" id="email" placeholder="Email">
            </div>
            <div class="mb-3">
              <textarea class="form-control" v-model="message" id="message" placeholder="Message" rows="10"></textarea>
            </div>

            <div class="d-flex">
              <span>
                <i class="fa fa-2x fa-linkedin"></i>
                <i class="fa fa-2x fa-gitlab"></i>
                <i class="fa fa-2x fa-github"></i>
              </span>
              <button type="submit" class="btn btn-primary ms-auto">Submit</button>
            </div>

            <!-- reCaptcha Checkbox -->
            <div class="d-flex justify-content-end mt-2">
              <div ref="recaptchaContainer"></div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>