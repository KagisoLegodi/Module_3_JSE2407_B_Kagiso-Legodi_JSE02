<script>
    // @ts-ignore
    import { Link } from 'svelte-routing';
    let showNavbar = false;
  
    const toggleNavbar = () => {
      showNavbar = !showNavbar;
    };
  </script>

  <header class="sticky top-0 z-50">
    <nav class="bg-red-500 border-yellow-200">
        <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
            <Link to="/">
                <div class="flex items-center space-x-3">
                  <img src="/online-shop.png" class="h-8" alt="SwiftCart Logo" />
                  <span class="self-center text-2xl font-semibold whitespace-nowrap text-white">SwiftCart</span>
                </div>
              </Link>
        </div>
    </nav>
  </header>