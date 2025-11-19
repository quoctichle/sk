<template>
  <div style="min-height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, #f5f1e8 0%, #e8dcc8 100%);">
    <main style="text-align: center; max-width: 600px; padding: 20px;">
      <div style="display: flex; gap: 20px; justify-content: center;">
        <a href="/vi" style="background: linear-gradient(135deg, #52a94b 0%, #7fb069 100%); color: white; padding: 15px 40px; border-radius: 25px; text-decoration: none; font-weight: bold; font-size: 1.1rem; transition: transform 0.3s; cursor: pointer;" @mouseover="$event.target.style.transform='scale(1.05)'" @mouseout="$event.target.style.transform='scale(1)';">Tiếng Việt</a>
        <a href="/en" style="background: linear-gradient(135deg, #52a94b 0%, #7fb069 100%); color: white; padding: 15px 40px; border-radius: 25px; text-decoration: none; font-weight: bold; font-size: 1.1rem; transition: transform 0.3s; cursor: pointer;" @mouseover="$event.target.style.transform='scale(1.05)'" @mouseout="$event.target.style.transform='scale(1)';">English</a>
      </div>
    </main>
  </div>
</template>
