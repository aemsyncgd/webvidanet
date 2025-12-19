// Inicializar AOS
AOS.init({
    duration: 1000,
    once: true,
    easing: 'ease-out-cubic',
    offset: 100,
    delay: 100
});

// Inicializar Swiper
const swiper = new Swiper('.plansSwiper', {
    slidesPerView: 1,
    spaceBetween: 30,
    loop: false,
    pagination: {
        el: '.swiper-pagination',
        clickable: true,
    },
    navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
    },
    breakpoints: {
        768: {
            slidesPerView: 2,
        },
        1024: {
            slidesPerView: 3,
        },
    },
});

// Variables globales
const body = document.body;
const themeToggle = document.getElementById('themeToggle');
const hamburgerBtn = document.getElementById('hamburgerBtn');
const navList = document.getElementById('navList');
const currentYearSpan = document.getElementById('currentYear');
const header = document.querySelector('.header');
const navLinks = document.querySelectorAll('.nav-link');
const speedGauge = document.getElementById('speedGauge');
const gaugeValue = document.getElementById('gaugeValue');

// Año actual
currentYearSpan.textContent = new Date().getFullYear();

// Menú hamburguesa
hamburgerBtn.addEventListener('click', () => {
    navList.classList.toggle('active');
    hamburgerBtn.setAttribute('aria-expanded', navList.classList.contains('active'));
    hamburgerBtn.innerHTML = navList.classList.contains('active')
        ? '<i class="fas fa-times"></i>'
        : '<i class="fas fa-bars"></i>';
});

// Cerrar menú al hacer clic en un enlace
navLinks.forEach(link => {
    link.addEventListener('click', () => {
        navList.classList.remove('active');
        hamburgerBtn.setAttribute('aria-expanded', 'false');
        hamburgerBtn.innerHTML = '<i class="fas fa-bars"></i>';
    });
});

// Sistema de tema
function getThemePreference() {
    const storedTheme = localStorage.getItem('vidanet-theme');
    if (storedTheme) return storedTheme;
    return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
}

function setTheme(theme) {
    body.className = theme === 'dark' ? 'dark-theme' : 'light-theme';
    localStorage.setItem('vidanet-theme', theme);
    themeToggle.setAttribute('aria-label', theme === 'dark' ? 'Cambiar a tema claro' : 'Cambiar a tema oscuro');

    // Disparar evento para que otros componentes se actualicen
    window.dispatchEvent(new CustomEvent('themechange', { detail: { theme } }));
}

function toggleTheme() {
    const currentTheme = body.classList.contains('dark-theme') ? 'dark' : 'light';
    const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
    setTheme(newTheme);

    // Animación del botón
    themeToggle.style.transform = 'scale(0.8) rotate(180deg)';
    setTimeout(() => {
        themeToggle.style.transform = 'scale(1) rotate(0)';
    }, 300);
}

// Inicializar tema
setTheme(getThemePreference());

// Eventos del tema
themeToggle.addEventListener('click', toggleTheme);
themeToggle.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
        e.preventDefault();
        toggleTheme();
    }
});

// Header con scroll
window.addEventListener('scroll', () => {
    if (window.scrollY > 100) {
        header.classList.add('scrolled');
    } else {
        header.classList.remove('scrolled');
    }
});

// Smooth scroll con offset
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        e.preventDefault();

        const targetId = this.getAttribute('href');
        if (targetId === '#') return;

        const targetElement = document.querySelector(targetId);
        if (targetElement) {
            const headerHeight = header.offsetHeight;
            const targetPosition = targetElement.getBoundingClientRect().top + window.pageYOffset - headerHeight;

            window.scrollTo({
                top: targetPosition,
                behavior: 'smooth'
            });

            // Actualizar link activo
            navLinks.forEach(link => link.classList.remove('active'));
            this.classList.add('active');
        }
    });
});

// Observer para actualizar links activos
const sections = document.querySelectorAll('section[id]');
const observerOptions = {
    root: null,
    rootMargin: '-50% 0px -50% 0px',
    threshold: 0
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const id = entry.target.getAttribute('id');
            navLinks.forEach(link => {
                link.classList.remove('active');
                if (link.getAttribute('href') === `#${id}`) {
                    link.classList.add('active');
                }
            });
        }
    });
}, observerOptions);

sections.forEach(section => observer.observe(section));

// Gauge Animation
function animateGauge() {
    const canvas = speedGauge;
    const ctx = canvas.getContext('2d');
    const centerX = canvas.width / 2;
    const centerY = canvas.height / 2;
    const radius = Math.min(centerX, centerY) - 30;

    let startTime = null;
    const duration = 1800;
    const maxSpeed = 1000;

    // Easing function
    function easeOutElastic(t) {
        return t === 0 ? 0 :
            t === 1 ? 1 :
                Math.pow(2, -10 * t) * Math.sin((t * 10 - 0.75) * (2 * Math.PI) / 3) + 1;
    }

    function drawGauge(progress) {
        // Limpiar canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Obtener colores
        const primaryColor = getComputedStyle(document.documentElement).getPropertyValue('--color-primary').trim();
        const secondaryColor = getComputedStyle(document.documentElement).getPropertyValue('--color-secondary').trim();

        // Calcular ángulo
        const startAngle = -Math.PI;
        const endAngle = 0;
        const currentAngle = startAngle + (endAngle - startAngle) * progress;

        // Dibujar arco de fondo
        ctx.beginPath();
        ctx.arc(centerX, centerY, radius, startAngle, endAngle);
        ctx.lineWidth = 25;
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.1)';
        ctx.stroke();

        // Crear gradiente
        const gradient = ctx.createLinearGradient(0, centerY, canvas.width, centerY);
        gradient.addColorStop(0, primaryColor);
        gradient.addColorStop(1, secondaryColor);

        // Dibujar arco de progreso
        ctx.beginPath();
        ctx.arc(centerX, centerY, radius, startAngle, currentAngle);
        ctx.lineWidth = 25;
        ctx.strokeStyle = gradient;
        ctx.lineCap = 'round';
        ctx.stroke();

        // Dibujar marcas
        for (let i = 0; i <= 1000; i += 100) {
            const angle = startAngle + (endAngle - startAngle) * (i / 1000);
            const x1 = centerX + (radius - 15) * Math.cos(angle);
            const y1 = centerY + (radius - 15) * Math.sin(angle);
            const x2 = centerX + (radius - 5) * Math.cos(angle);
            const y2 = centerY + (radius - 5) * Math.sin(angle);

            ctx.beginPath();
            ctx.moveTo(x1, y1);
            ctx.lineTo(x2, y2);
            ctx.lineWidth = i % 200 === 0 ? 3 : 1;
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
            ctx.stroke();

            // Etiquetas
            if (i % 200 === 0) {
                const labelX = centerX + (radius - 35) * Math.cos(angle);
                const labelY = centerY + (radius - 35) * Math.sin(angle);

                ctx.font = '14px Poppins';
                ctx.fillStyle = 'rgba(255, 255, 255, 0.7)';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(i.toString(), labelX, labelY);
            }
        }

        // Dibujar aguja
        const needleLength = radius - 40;
        const needleX = centerX + needleLength * Math.cos(currentAngle);
        const needleY = centerY + needleLength * Math.sin(currentAngle);

        ctx.beginPath();
        ctx.moveTo(centerX, centerY);
        ctx.lineTo(needleX, needleY);
        ctx.lineWidth = 4;
        ctx.strokeStyle = '#FFFFFF';
        ctx.stroke();

        // Centro de la aguja
        ctx.beginPath();
        ctx.arc(centerX, centerY, 10, 0, Math.PI * 2);
        ctx.fillStyle = '#FFFFFF';
        ctx.fill();
    }

    function animate(timestamp) {
        if (!startTime) startTime = timestamp;
        const elapsed = timestamp - startTime;
        const progress = Math.min(elapsed / duration, 1);
        const easedProgress = easeOutElastic(progress);

        // Calcular valor actual
        const currentSpeed = Math.round(maxSpeed * easedProgress);

        drawGauge(easedProgress);
        gaugeValue.textContent = currentSpeed;

        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            // Valor final exacto
            gaugeValue.textContent = maxSpeed;
            drawGauge(1);
        }
    }

    // Iniciar animación cuando el gauge entra en el viewport
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                startTime = null;
                requestAnimationFrame(animate);
                observer.unobserve(speedGauge);
            }
        });
    }, { threshold: 0.5 });

    observer.observe(speedGauge);
}

// Iniciar gauge cuando la página cargue
window.addEventListener('load', () => {
    setTimeout(animateGauge, 500);
});

// Redibujar gauge cuando cambia el tema
window.addEventListener('themechange', () => {
    setTimeout(() => {
        const currentValue = parseInt(gaugeValue.textContent);
        const canvas = speedGauge;
        const ctx = canvas.getContext('2d');
        const centerX = canvas.width / 2;
        const centerY = canvas.height / 2;
        const radius = Math.min(centerX, centerY) - 30;
        const progress = currentValue / 1000;

        // Redibujar con nuevos colores
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        const primaryColor = getComputedStyle(document.documentElement).getPropertyValue('--color-primary').trim();
        const secondaryColor = getComputedStyle(document.documentElement).getPropertyValue('--color-secondary').trim();

        const startAngle = -Math.PI;
        const endAngle = 0;
        const currentAngle = startAngle + (endAngle - startAngle) * progress;

        // Arco de fondo
        ctx.beginPath();
        ctx.arc(centerX, centerY, radius, startAngle, endAngle);
        ctx.lineWidth = 25;
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.1)';
        ctx.stroke();

        // Gradiente
        const gradient = ctx.createLinearGradient(0, centerY, canvas.width, centerY);
        gradient.addColorStop(0, primaryColor);
        gradient.addColorStop(1, secondaryColor);

        // Dibujar arco de progreso
        ctx.beginPath();
        ctx.arc(centerX, centerY, radius, startAngle, currentAngle);
        ctx.lineWidth = 25;
        ctx.strokeStyle = gradient;
        ctx.lineCap = 'round';
        ctx.stroke();

        // Dibujar marcas
        for (let i = 0; i <= 1000; i += 100) {
            const angle = startAngle + (endAngle - startAngle) * (i / 1000);
            const x1 = centerX + (radius - 15) * Math.cos(angle);
            const y1 = centerY + (radius - 15) * Math.sin(angle);
            const x2 = centerX + (radius - 5) * Math.cos(angle);
            const y2 = centerY + (radius - 5) * Math.sin(angle);

            ctx.beginPath();
            ctx.moveTo(x1, y1);
            ctx.lineTo(x2, y2);
            ctx.lineWidth = i % 200 === 0 ? 3 : 1;
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
            ctx.stroke();

            // Etiquetas
            if (i % 200 === 0) {
                const labelX = centerX + (radius - 35) * Math.cos(angle);
                const labelY = centerY + (radius - 35) * Math.sin(angle);

                ctx.font = '14px Poppins';
                ctx.fillStyle = 'rgba(255, 255, 255, 0.7)';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(i.toString(), labelX, labelY);
            }
        }

        // Aguja
        const needleLength = radius - 40;
        const needleX = centerX + needleLength * Math.cos(currentAngle);
        const needleY = centerY + needleLength * Math.sin(currentAngle);

        ctx.beginPath();
        ctx.moveTo(centerX, centerY);
        ctx.lineTo(needleX, needleY);
        ctx.lineWidth = 4;
        ctx.strokeStyle = '#FFFFFF';
        ctx.stroke();

        // Centro
        ctx.beginPath();
        ctx.arc(centerX, centerY, 10, 0, Math.PI * 2);
        ctx.fillStyle = '#FFFFFF';
        ctx.fill();
    }, 100);
});
