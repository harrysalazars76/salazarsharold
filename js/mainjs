document.addEventListener('DOMContentLoaded', function () {
  // Animated count-up for hero stats
  var counters = Array.prototype.slice.call(document.querySelectorAll('[data-count-to]'));
  if (counters.length) {
    var animated = false;
    function easeOutQuad(t) { return t * (2 - t); }
    function runCounters() {
      if (animated) return;
      animated = true;
      counters.forEach(function (el) {
        var target = parseFloat(el.getAttribute('data-count-to'), 10);
        var suffix = el.getAttribute('data-suffix') || '';
        var duration = 1400;
        var start = null;
        function step(ts) {
          if (!start) start = ts;
          var progress = Math.min((ts - start) / duration, 1);
          var value = Math.round(target * easeOutQuad(progress));
          el.textContent = value + suffix;
          if (progress < 1) requestAnimationFrame(step);
        }
        requestAnimationFrame(step);
      });
    }
    var statsBlock = document.querySelector('.hero-stats');
    if (statsBlock && 'IntersectionObserver' in window) {
      var io = new IntersectionObserver(function (entries) {
        entries.forEach(function (entry) {
          if (entry.isIntersecting) { runCounters(); io.disconnect(); }
        });
      }, { threshold: 0.3 });
      io.observe(statsBlock);
    } else {
      runCounters();
    }
  }

  // Contact dropdown (WhatsApp / Llamar / Correo + redes sociales)
  var contactDropdowns = Array.prototype.slice.call(document.querySelectorAll('.contact-dropdown'));
  contactDropdowns.forEach(function (dd) {
    var btn = dd.querySelector('.header-cta');
    if (!btn) return;
    btn.addEventListener('click', function (e) {
      e.stopPropagation();
      var isOpen = dd.classList.contains('open');
      contactDropdowns.forEach(function (other) {
        other.classList.remove('open');
        var b = other.querySelector('.header-cta');
        if (b) b.setAttribute('aria-expanded', 'false');
      });
      if (!isOpen) {
        dd.classList.add('open');
        btn.setAttribute('aria-expanded', 'true');
      }
    });
  });
  document.addEventListener('click', function (e) {
    contactDropdowns.forEach(function (dd) {
      if (!dd.contains(e.target)) {
        dd.classList.remove('open');
        var b = dd.querySelector('.header-cta');
        if (b) b.setAttribute('aria-expanded', 'false');
      }
    });
  });
  document.addEventListener('keydown', function (e) {
    if (e.key === 'Escape') {
      contactDropdowns.forEach(function (dd) {
        dd.classList.remove('open');
        var b = dd.querySelector('.header-cta');
        if (b) b.setAttribute('aria-expanded', 'false');
      });
    }
  });

  // Mobile nav toggle
  var toggle = document.querySelector('.nav-toggle');
  var nav = document.querySelector('.main-nav');
  if (toggle && nav) {
    toggle.addEventListener('click', function () {
      nav.classList.toggle('open');
    });
    nav.querySelectorAll('a').forEach(function (a) {
      a.addEventListener('click', function () { nav.classList.remove('open'); });
    });
  }

  // Simple lightbox for .gallery-item elements
  var items = Array.prototype.slice.call(document.querySelectorAll('.gallery-item[data-full]'));
  if (items.length) {
    var overlay = document.createElement('div');
    overlay.className = 'lightbox-overlay';
    overlay.innerHTML =
      '<button class="lightbox-close" aria-label="Cerrar">&times;</button>' +
      '<button class="lightbox-nav lightbox-prev" aria-label="Anterior">&#8249;</button>' +
      '<img src="" alt="">' +
      '<button class="lightbox-nav lightbox-next" aria-label="Siguiente">&#8250;</button>';
    document.body.appendChild(overlay);
    var img = overlay.querySelector('img');
    var idx = 0;

    function show(i) {
      idx = (i + items.length) % items.length;
      img.src = items[idx].getAttribute('data-full');
      img.alt = items[idx].getAttribute('data-label') || '';
    }
    items.forEach(function (el, i) {
      el.addEventListener('click', function () {
        show(i);
        overlay.classList.add('open');
      });
    });
    overlay.querySelector('.lightbox-close').addEventListener('click', function () {
      overlay.classList.remove('open');
    });
    overlay.querySelector('.lightbox-prev').addEventListener('click', function () { show(idx - 1); });
    overlay.querySelector('.lightbox-next').addEventListener('click', function () { show(idx + 1); });
    overlay.addEventListener('click', function (e) {
      if (e.target === overlay) overlay.classList.remove('open');
    });
    document.addEventListener('keydown', function (e) {
      if (!overlay.classList.contains('open')) return;
      if (e.key === 'Escape') overlay.classList.remove('open');
      if (e.key === 'ArrowLeft') show(idx - 1);
      if (e.key === 'ArrowRight') show(idx + 1);
    });
  }
});
