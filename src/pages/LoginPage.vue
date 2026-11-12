<script setup>
import { computed, ref } from "vue";

const email = ref("");
const password = ref("");
const rememberMe = ref(true);
const showPassword = ref(false);
const isSubmitting = ref(false);
const feedback = ref("");
const feedbackType = ref("danger");

const apiBaseUrl = (
  import.meta.env.VITE_API_BASE_URL || "http://localhost:3000/api/v1"
).replace(/\/+$/, "");

const passwordInputType = computed(() =>
  showPassword.value ? "text" : "password",
);

async function submitLogin() {
  feedback.value = "";
  isSubmitting.value = true;

  try {
    const response = await fetch(`${apiBaseUrl}/auth/signin`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        email: email.value.trim(),
        password: password.value,
        rememberMe: rememberMe.value,
      }),
    });
    const result = await response.json().catch(() => null);

    if (!response.ok) {
      throw new Error(
        result?.message || "Email atau kata sandi tidak dapat diverifikasi.",
      );
    }

    feedbackType.value = "success";
    feedback.value = "Berhasil masuk.";
  } catch (error) {
    feedbackType.value = "danger";
    feedback.value =
      error instanceof TypeError
        ? "Tidak dapat terhubung ke server. Pastikan API sudah berjalan."
        : error.message;
  } finally {
    isSubmitting.value = false;
  }
}

function showRecoveryMessage() {
  feedbackType.value = "info";
  feedback.value =
    "Silakan hubungi administrator untuk bantuan pemulihan kata sandi.";
}
</script>

<template>
  <main class="login-shell">
    <section class="welcome-panel" aria-label="Tentang QDAS">
      <a class="brand brand-light" href="/login" aria-label="QDAS beranda">
        <span class="brand-mark" aria-hidden="true">
          <svg viewBox="0 0 40 40" fill="none">
            <path
              d="M7 30.5V12.2L20 6l13 6.2v18.3M13 30.5V17.2l7-3.4 7 3.4v13.3M4 33.5h32"
              stroke="currentColor"
              stroke-width="2.4"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path d="M18 22h4m-2-2v8" stroke="currentColor" stroke-width="2" />
          </svg>
        </span>
        <span class="brand-copy">
          <strong>QDAS</strong>
          <span>INTERVENSI PEMERINTAH</span>
        </span>
      </a>

      <div class="welcome-content">
        <span class="eyebrow"><span></span> SATU DATA, DAMPAK NYATA</span>
        <h1>
          Bersama membangun
          <span>intervensi yang tepat.</span>
        </h1>
        <p>
          Platform data terpadu untuk mendukung perencanaan, pemantauan, dan
          evaluasi program intervensi pemerintah.
        </p>

        <div class="visual-card" aria-hidden="true">
          <div class="visual-card-header">
            <span>Ringkasan Intervensi</span>
            <span class="live-indicator">REAL-TIME</span>
          </div>
          <div class="visual-chart">
            <div class="chart-grid">
              <span></span><span></span><span></span><span></span>
            </div>
            <svg viewBox="0 0 460 148" preserveAspectRatio="none">
              <defs>
                <linearGradient id="chart-fill" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="#7bd9c6" stop-opacity=".28" />
                  <stop offset="100%" stop-color="#7bd9c6" stop-opacity="0" />
                </linearGradient>
              </defs>
              <path
                d="M0 119C30 113 31 79 66 86s41 37 74 21 41-53 76-43 44 38 76 16 42-46 78-31 45 38 90-29v128H0Z"
                fill="url(#chart-fill)"
              />
              <path
                d="M0 119C30 113 31 79 66 86s41 37 74 21 41-53 76-43 44 38 76 16 42-46 78-31 45 38 90-29"
                fill="none"
                stroke="#7bd9c6"
                stroke-linecap="round"
                stroke-width="3"
              />
            </svg>
          </div>
          <div class="chart-legend">
            <span><i></i> Capaian program</span>
            <strong>+18,6% <small>tahun ini</small></strong>
          </div>
        </div>
      </div>

      <div class="welcome-footer">
        <span>Terintegrasi untuk Indonesia yang lebih baik</span>
        <span class="footer-seal" aria-hidden="true">
          <svg viewBox="0 0 24 24" fill="none">
            <path
              d="M12 3 19 6v5c0 4.5-2.9 8-7 10-4.1-2-7-5.5-7-10V6l7-3Z"
              stroke="currentColor"
              stroke-width="1.5"
              stroke-linejoin="round"
            />
            <path
              d="m8.7 12 2.1 2.1 4.7-4.7"
              stroke="currentColor"
              stroke-width="1.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </span>
      </div>
    </section>

    <section class="form-panel" aria-labelledby="login-title">
      <div class="mobile-brand">
        <a class="brand" href="/login">
          <span class="brand-mark" aria-hidden="true">
            <svg viewBox="0 0 40 40" fill="none">
              <path
                d="M7 30.5V12.2L20 6l13 6.2v18.3M13 30.5V17.2l7-3.4 7 3.4v13.3M4 33.5h32"
                stroke="currentColor"
                stroke-width="2.4"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
              <path
                d="M18 22h4m-2-2v8"
                stroke="currentColor"
                stroke-width="2"
              />
            </svg>
          </span>
          <span class="brand-copy">
            <strong>QDAS</strong>
            <span>INTERVENSI PEMERINTAH</span>
          </span>
        </a>
      </div>

      <div class="login-card">
        <div class="login-heading">
          <span class="eyebrow form-eyebrow">PORTAL RESMI</span>
          <h2 id="login-title">Selamat datang</h2>
          <p>Masuk untuk melanjutkan ke dashboard QDAS Anda.</p>
        </div>

        <form @submit.prevent="submitLogin">
          <div class="mb-4">
            <label for="email" class="form-label">Email</label>
            <div class="input-wrap">
              <svg viewBox="0 0 20 20" fill="none" aria-hidden="true">
                <rect
                  x="2.25"
                  y="3.75"
                  width="15.5"
                  height="12.5"
                  rx="2.25"
                  stroke="currentColor"
                  stroke-width="1.5"
                />
                <path
                  d="m3 5 7 5.5L17 5"
                  stroke="currentColor"
                  stroke-width="1.5"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              <input
                id="email"
                v-model="email"
                class="form-control"
                type="email"
                name="email"
                placeholder="nama@instansi.go.id"
                autocomplete="username"
                required
              />
            </div>
          </div>

          <div class="mb-3">
            <div class="label-row">
              <label for="password" class="form-label">Kata sandi</label>
              <button
                class="text-action"
                type="button"
                @click="showRecoveryMessage"
              >
                Lupa kata sandi?
              </button>
            </div>
            <div class="input-wrap">
              <svg viewBox="0 0 20 20" fill="none" aria-hidden="true">
                <rect
                  x="3.25"
                  y="8.25"
                  width="13.5"
                  height="9.5"
                  rx="2"
                  stroke="currentColor"
                  stroke-width="1.5"
                />
                <path
                  d="M6.25 8V5.75a3.75 3.75 0 0 1 7.5 0V8M10 12v2"
                  stroke="currentColor"
                  stroke-width="1.5"
                  stroke-linecap="round"
                />
              </svg>
              <input
                id="password"
                v-model="password"
                class="form-control password-input"
                :type="passwordInputType"
                name="password"
                placeholder="Masukkan kata sandi"
                autocomplete="current-password"
                required
              />
              <button
                class="password-toggle"
                type="button"
                :aria-label="showPassword ? 'Sembunyikan kata sandi' : 'Lihat kata sandi'"
                @click="showPassword = !showPassword"
              >
                <svg
                  v-if="showPassword"
                  viewBox="0 0 20 20"
                  fill="none"
                  aria-hidden="true"
                >
                  <path
                    d="M2.5 10s2.7-5 7.5-5 7.5 5 7.5 5-2.7 5-7.5 5-7.5-5-7.5-5Z"
                    stroke="currentColor"
                    stroke-width="1.5"
                  />
                  <circle
                    cx="10"
                    cy="10"
                    r="2"
                    stroke="currentColor"
                    stroke-width="1.5"
                  />
                </svg>
                <svg
                  v-else
                  viewBox="0 0 20 20"
                  fill="none"
                  aria-hidden="true"
                >
                  <path
                    d="M3 3 17 17M8.6 5.2c.45-.13.92-.2 1.4-.2 4.8 0 7.5 5 7.5 5a12 12 0 0 1-2 2.65M5.2 5.8C3.45 7 2.5 10 2.5 10s2.7 5 7.5 5c.8 0 1.55-.16 2.25-.42"
                    stroke="currentColor"
                    stroke-width="1.5"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </button>
            </div>
          </div>

          <div class="remember-row">
            <div class="form-check">
              <input
                id="remember"
                v-model="rememberMe"
                class="form-check-input"
                type="checkbox"
              />
              <label class="form-check-label" for="remember">Ingat saya</label>
            </div>
            <span class="secure-note">
              <svg viewBox="0 0 16 16" fill="none" aria-hidden="true">
                <path
                  d="m8 1.8 5 2.1v3.6c0 3.2-2.1 5.7-5 7.1-2.9-1.4-5-3.9-5-7.1V3.9l5-2.1Z"
                  stroke="currentColor"
                  stroke-width="1.2"
                  stroke-linejoin="round"
                />
                <path
                  d="m5.8 8 1.5 1.5 2.9-3"
                  stroke="currentColor"
                  stroke-width="1.2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              Koneksi aman
            </span>
          </div>

          <div
            v-if="feedback"
            class="alert login-alert"
            :class="`alert-${feedbackType}`"
            role="status"
          >
            {{ feedback }}
          </div>

          <button
            class="btn btn-primary login-button"
            type="submit"
            :disabled="isSubmitting"
          >
            <span
              v-if="isSubmitting"
              class="spinner-border spinner-border-sm"
              aria-hidden="true"
            ></span>
            {{ isSubmitting ? "Memverifikasi..." : "Masuk ke akun" }}
            <svg
              v-if="!isSubmitting"
              viewBox="0 0 20 20"
              fill="none"
              aria-hidden="true"
            >
              <path
                d="M4 10h12m-5-5 5 5-5 5"
                stroke="currentColor"
                stroke-width="1.6"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </button>
        </form>

        <div class="help-card">
          <span class="help-icon" aria-hidden="true">
            <svg viewBox="0 0 20 20" fill="none">
              <path
                d="M3 4.75h14v9.5H9l-4.25 3v-3H3v-9.5Z"
                stroke="currentColor"
                stroke-width="1.5"
                stroke-linejoin="round"
              />
              <path
                d="M7 8h6m-6 3h4"
                stroke="currentColor"
                stroke-width="1.5"
                stroke-linecap="round"
              />
            </svg>
          </span>
          <span>
            <strong>Butuh bantuan?</strong>
            <small>Hubungi administrator instansi Anda.</small>
          </span>
        </div>
      </div>

      <footer class="form-footer">
        <span>© 2026 QDAS Intervensi Pemerintah</span>
        <span class="footer-links">
          <span>Pusat bantuan</span>
          <span aria-hidden="true">·</span>
          <span>v1.0.0</span>
        </span>
      </footer>
    </section>
  </main>
</template>
