// Мобильное меню и взаимодействия
document.addEventListener('DOMContentLoaded', function() {
    const menuToggle = document.getElementById('menuToggle');
    const navList = document.getElementById('navList');
    const dropdowns = document.querySelectorAll('.dropdown');

    // Переключение основного меню
    menuToggle.addEventListener('click', function() {
        this.classList.toggle('active');
        navList.classList.toggle('active');
        
        // Закрываем все выпадающие меню при закрытии основного
        if (!navList.classList.contains('active')) {
            closeAllDropdowns();
        }
    });

    // Обработка выпадающих меню на мобильных
    dropdowns.forEach(dropdown => {
        const link = dropdown.querySelector('.nav-link');
        const menu = dropdown.querySelector('.dropdown-menu');
        const icon = dropdown.querySelector('.dropdown-icon');

        link.addEventListener('click', function(e) {
            if (window.innerWidth <= 768) {
                e.preventDefault();
                e.stopPropagation();
                
                // Закрываем другие открытые меню
                closeOtherDropdowns(dropdown);
                
                // Переключаем текущее меню
                menu.classList.toggle('active');
                if (icon) {
                    icon.style.transform = menu.classList.contains('active') 
                        ? 'rotate(180deg)' 
                        : 'rotate(0deg)';
                }
            }
        });
    });

    // Закрытие меню при клике вне его
    document.addEventListener('click', function(e) {
        if (window.innerWidth <= 768) {
            if (!e.target.closest('.navbar') && navList.classList.contains('active')) {
                closeMobileMenu();
            }
        }
    });

    // Плавная прокрутка для якорных ссылок
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function (e) {
            e.preventDefault();
            const targetId = this.getAttribute('href');
            if (targetId === '#') return;
            
            const targetElement = document.querySelector(targetId);
            if (targetElement) {
                // Закрываем мобильное меню если открыто
                if (window.innerWidth <= 768) {
                    closeMobileMenu();
                }
                
                const headerHeight = document.querySelector('.header').offsetHeight;
                const targetPosition = targetElement.getBoundingClientRect().top + window.pageYOffset - headerHeight;
                
                window.scrollTo({
                    top: targetPosition,
                    behavior: 'smooth'
                });
            }
        });
    });

    // Адаптация к изменению размера окна
    window.addEventListener('resize', function() {
        if (window.innerWidth > 768) {
            closeMobileMenu();
        }
    });

    // Функции помощники
    function closeMobileMenu() {
        menuToggle.classList.remove('active');
        navList.classList.remove('active');
        closeAllDropdowns();
    }

    function closeAllDropdowns() {
        dropdowns.forEach(dropdown => {
            const menu = dropdown.querySelector('.dropdown-menu');
            const icon = dropdown.querySelector('.dropdown-icon');
            menu.classList.remove('active');
            if (icon) icon.style.transform = 'rotate(0deg)';
        });
    }

    function closeOtherDropdowns(currentDropdown) {
        dropdowns.forEach(dropdown => {
            if (dropdown !== currentDropdown) {
                const menu = dropdown.querySelector('.dropdown-menu');
                const icon = dropdown.querySelector('.dropdown-icon');
                menu.classList.remove('active');
                if (icon) icon.style.transform = 'rotate(0deg)';
            }
        });
    }
// Анимация появления элементов при скролле
    const animateOnScroll = function() {
        const elements = document.querySelectorAll('.service-card, .review-card, .feature');
        
        elements.forEach(element => {
            const elementTop = element.getBoundingClientRect().top;
            const elementVisible = 150;
            
            if (elementTop < window.innerHeight - elementVisible) {
                element.style.opacity = "1";
                element.style.transform = "translateY(0)";
            }
        });
    };

    // Установка начальных стилей для анимации
    const animatedElements = document.querySelectorAll('.service-card, .review-card, .feature');
    animatedElements.forEach(element => {
        element.style.opacity = "0";
        element.style.transform = "translateY(30px)";
        element.style.transition = "opacity 0.6s ease, transform 0.6s ease";
    });

    // Запуск анимации при загрузке и скролле
    window.addEventListener('load', animateOnScroll);
    window.addEventListener('scroll', animateOnScroll);
});