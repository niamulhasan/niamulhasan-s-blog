<template>
  <div class="w-full shadow-sm scrim-bg fixed z-40 top-0 inset-x-0">
    <nav
      class="
        flex
        w-full
        items-center
        justify-between
        flex-wrap
        bg-gray-800
        py-0
        mx-auto
        px-8
        pt-5
        pb-2
        lg:pt-0
        lg:pb-0
        header-dark-fix
      "
    >
      <div class="flex items-center flex-shrink-0 text-white mr-6">
        <a class="flex item-center text-white no-underline hover:text-white hover:no-underline pl-2" href="#">
          <div class="h-8 text-blue-500 w-max block mx-auto rounded-sm text-bn text-xl">নিয়ামুল হাসানের ব্লগ</div>
        </a>
      </div>

      <div class="block lg:hidden">
        <button id="nav-toggle" class="focus:outline-none">
          <div class="h-1 bg-gray-300 w-8 mb-1 block mx-auto rounded-sm"></div>
          <div class="h-1 bg-gray-300 w-8 mb-1 block mx-auto rounded-sm"></div>
          <div class="h-1 bg-gray-300 w-8 mb-1 block mx-auto rounded-sm"></div>
        </button>
      </div>

      <div class="w-full flex-grow lg:flex lg:items-center lg:w-auto hidden lg:block pt-6 lg:pt-0" id="nav-content">
        <ul class="list-reset lg:flex justify-end flex-1 items-center">
          <li class="mr-3">
            <nuxt-link class="inline-block py-4 px-6 active:text-gray-900 no-underline py-4 text-bn" to="/"
              >নীড়পাতা</nuxt-link
            >
          </li>
          <li class="mr-3">
            <nuxt-link class="inline-block py-4 px-6 active:text-gray-900 no-underline py-4 text-bn" to="/blog">
              ব্লগ পাতা
            </nuxt-link>
          </li>
          <li class="mr-3">
            <nuxt-link class="inline-block py-4 px-6 active:text-gray-900 no-underline py-4 text-bn" to="/projects">
              সাহিত্য পাতা
            </nuxt-link>
          </li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: 'Header',
}
</script>

<style lang="postcss" scoped>
.light .header-dark-fix {
  background: #fff;
}

.scrim-bg {
  &::before {
    content: '';
    z-index: -1;
    background-color: var(--bg);
    @apply absolute bottom-0 inset-x-0 h-12 mb-4 transition-colors duration-200 ease-in-out;
  }
  &::after {
    content: '';
    z-index: -1;
    opacity: 1;
    animation: fadeIn1 500ms ease-in-out;
    @apply pointer-events-none absolute bottom-0 inset-x-0 h-16 -mb-12;
    background: linear-gradient(to bottom, #111827, cubic-bezier(0.15, 0, 0.45, 1), transparent);
  }
}
.nuxt-link-exact-active {
  @apply text-blue-600 border-b-2 border-blue-600 hover:bg-blue-100 transition duration-300;
}

.light {
  & .scrim-bg {
    &::after {
      animation-name: fadeIn2;
      background: linear-gradient(to bottom, #e5e7eb, cubic-bezier(0.15, 0, 0.45, 1), transparent);
    }
  }
  & .nuxt-link-exact-active {
    @apply text-blue-600 border-b-2 border-blue-600 bg-blue-100 transition duration-300;
  }
}

/* Need two because of smoother switching between color modes */
@keyframes fadeIn1 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes fadeIn2 {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
