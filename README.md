Hello, welcome. We are a company with 20 years of experience.import React, { useState } from "react";

// CleaningCompany - Single-file React component (Tailwind CSS)
// - Tailwind must be configured in the host project.
// - To enable payments: implement a server endpoint POST /api/create-checkout-session
//   that creates a Stripe Checkout Session (server-side) and returns { url }.
// - This file is a starting template: customize copy, images, prices and API keys.

export default function CleaningCompanySite() {
  const servicesList = [
    { id: 1, title: "Limpieza doméstica", desc: "Limpieza regular y a fondo para casas y pisos.", price: 45 },
    { id: 2, title: "Limpieza de oficinas", desc: "Mantenimientos, limpieza de zonas comunes y escritorios.", price: 60 },
    { id: 3, title: "Limpieza de alfombras", desc: "Lavado a vapor y extracción de manchas.", price: 35 },
    { id: 4, title: "Limpieza profunda (checkout)", desc: "Limpieza final por finalización de arrendamiento.", price: 120 },
    { id: 5, title: "Limpieza de cortinas", desc: "Limpieza profesional de cortinas y visillos.", price: 40 },
    { id: 6, title: "Limpieza a vapor", desc: "Tratamientos a vapor para higienizar tejidos y superficies.", price: 50 },
    { id: 7, title: "Limpieza de sofás y sillas", desc: "Desincrustado y lavado profundo de tapicería.", price: 30 },
    { id: 8, title: "Hornos y extractores", desc: "Limpieza profunda de hornos, campanas y extractores.", price: 55 },
  ];

  const [booking, setBooking] = useState({
    name: "",
    phone: "",
    email: "",
    address: "",
    date: "",
    time: "",
    serviceId: servicesList[0].id,
    notes: "",
  });

  const [loading, setLoading] = useState(false);
  const [message, setMessage] = useState(null);

  function handleChange(e) {
    const { name, value } = e.target;
    setBooking((s) => ({ ...s, [name]: value }));
  }

  async function handleBooking(e) {
    e.preventDefault();
    setLoading(true);
    setMessage(null);

    // Client-side: call your backend to create a Stripe Checkout session.
    // Backend must use secret Stripe key and create the session server-side.
    try {
      const res = await fetch("/api/create-checkout-session", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ booking }),
      });
      const data = await res.json();
      if (data.url) {
        // redirect to Stripe Checkout
        window.location.href = data.url;
      } else {
        setMessage("No se pudo iniciar el pago. Revise el servidor.");
      }
    } catch (err) {
      console.error(err);
      setMessage("Error conectando con el servidor. Intente más tarde.");
    }

    setLoading(false);
  }

  async function handleContact(e) {
    e.preventDefault();
    // For a quick demo we open mailto. Replace with API call to send emails / store leads.
    const subject = encodeURIComponent("Contacto desde web - Cleaning Company");
    const body = encodeURIComponent(
      `Nombre: ${booking.name}\nEmail: ${booking.email}\nTelefono: ${booking.phone}\nMensaje: ${booking.notes}`
    );
    window.location.href = `mailto:contacto@cleaningcompany.com?subject=${subject}&body=${body}`;
  }

  const selectedService = servicesList.find((s) => s.id === Number(booking.serviceId));

  return (
    <div className="min-h-screen bg-gray-50 text-gray-800">
      {/* Navbar */}
      <header className="bg-white shadow-sm">
        <div className="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 rounded-full bg-gradient-to-br from-blue-500 to-yellow-400 flex items-center justify-center text-white font-bold">CC</div>
            <div>
              <h1 className="font-bold text-lg">Cleaning Company</h1>
              <p className="text-sm text-gray-500">Limpieza profesional — casas, oficinas y tapicería</p>
            </div>
          </div>
          <nav className="hidden md:flex gap-6 text-sm">
            <a href="#servicios" className="hover:underline">Servicios</a>
            <a href="#reservas" className="hover:underline">Reservas</a>
            <a href="#contacto" className="hover:underline">Contacto</a>
          </nav>
        </div>
      </header>

      {/* Hero */}
      <section className="bg-gradient-to-r from-blue-50 to-yellow-50 py-12">
        <div className="max-w-6xl mx-auto px-6 grid md:grid-cols-2 gap-8 items-center">
          <div>
            <h2 className="text-3xl md:text-4xl font-extrabold mb-3">Limpieza profesional para tu hogar y oficina</h2>
            <p className="text-gray-600 mb-6">Servicio a medida: limpiezas regulares, limpiezas a fondo, lavado de alfombras y tapicería, limpieza de hornos y extractores. Reserva online y paga con tarjeta.</p>
            <div className="flex gap-3">
              <a href="#reservas" className="px-5 py-3 rounded-lg bg-blue-600 text-white font-semibold">Reservar ahora</a>
              <a href="#servicios" className="px-5 py-3 rounded-lg border border-gray-200">Ver servicios</a>
            </div>
          </div>
          <div className="p-6 bg-white rounded-2xl shadow-md">
            <h3 className="font-bold mb-3">Resumen rápido</h3>
            <ul className="space-y-2 text-sm">
              <li>✅ Equipo profesional y productos seguros</li>
              <li>✅ Reserva online en minutos</li>
              <li>✅ Pagos con tarjeta (Stripe)</li>
              <li>✅ Servicio disponible para casas y oficinas</li>
            </ul>
          </div>
        </div>
      </section>

      {/* Services */}
      <section id="servicios" className="max-w-6xl mx-auto px-6 py-12">
        <h3 className="text-2xl font-bold mb-6">Servicios</h3>
        <div className="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
          {servicesList.map((s) => (
            <article key={s.id} className="bg-white p-6 rounded-2xl shadow-sm">
              <h4 className="font-semibold text-lg mb-1">{s.title}</h4>
              <p className="text-sm text-gray-600 mb-4">{s.desc}</p>
              <div className="flex items-center justify-between">
                <div className="text-sm text-gray-700">Desde</div>
                <div className="font-bold">£{s.price}</div>
              </div>
            </article>
          ))}
        </div>
      </section>

      {/* Booking */}
      <section id="reservas" className="bg-white py-12">
        <div className="max-w-4xl mx-auto px-6">
          <h3 className="text-2xl font-bold mb-4">Reserva y paga online</h3>
          <div className="grid md:grid-cols-2 gap-6">
            <form onSubmit={handleBooking} className="space-y-4">
              <div>
                <label className="block text-sm">Nombre completo</label>
                <input name="name" required value={booking.name} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
              </div>

              <div className="flex gap-3">
                <div className="flex-1">
                  <label className="block text-sm">Teléfono</label>
                  <input name="phone" required value={booking.phone} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
                </div>
                <div className="flex-1">
                  <label className="block text-sm">Email</label>
                  <input name="email" type="email" required value={booking.email} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
                </div>
              </div>

              <div>
                <label className="block text-sm">Dirección</label>
                <input name="address" required value={booking.address} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
              </div>

              <div className="flex gap-3">
                <div className="flex-1">
                  <label className="block text-sm">Fecha</label>
                  <input name="date" type="date" required value={booking.date} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
                </div>
                <div className="flex-1">
                  <label className="block text-sm">Hora</label>
                  <input name="time" type="time" required value={booking.time} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
                </div>
              </div>

              <div>
                <label className="block text-sm">Servicio</label>
                <select name="serviceId" value={booking.serviceId} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border">
                  {servicesList.map((s) => (
                    <option key={s.id} value={s.id}>{s.title} — £{s.price}</option>
                  ))}
                </select>
              </div>

              <div>
                <label className="block text-sm">Notas</label>
                <textarea name="notes" value={booking.notes} onChange={handleChange} className="mt-1 w-full p-3 rounded-md border" />
              </div>

              <div className="flex items-center gap-4">
                <div className="text-sm">Total estimado:</div>
                <div className="font-bold text-lg">£{selectedService.price}</div>
              </div>

              <div>
                <button type="submit" className="w-full py-3 rounded-lg bg-yellow-400 font-semibold" disabled={loading}>
                  {loading ? "Procesando…" : "Pagar y reservar"}
                </button>
              </div>

              {message && <div className="text-sm text-red-600">{message}</div>}
            </form>

            <aside className="p-6 bg-gray-50 rounded-xl">
              <h4 className="font-semibold mb-3">Cómo funciona</h4>
              <ol className="list-decimal ml-5 text-sm text-gray-600 space-y-2">
                <li>Completa el formulario y selecciona fecha/hora.</li>
                <li>Serás redirigido a una pasarela de pago segura.</li>
                <li>Tras el pago recibirás confirmación por email y nuestro equipo te llamará si hace falta.</li>
              </ol>

              <div className="mt-6">
                <h5 className="font-semibold">Política de cancelación</h5>
                <p className="text-sm text-gray-600">Cancelaciones con 24h de antelación: reembolso completo. Menos de 24h: tarifa de cancelación del 50%.</p>
              </div>
            </aside>
          </div>
        </div>
      </section>

      {/* Contact */}
      <section id="contacto" className="max-w-6xl mx-auto px-6 py-12">
        <div className="grid md:grid-cols-2 gap-6">
          <div className="space-y-4">
            <h3 className="text-2xl font-bold">Contacto</h3>
            <p className="text-sm text-gray-600">Tel: +44 7000 000000 | Email: contacto@cleaningcompany.com</p>
            <p className="text-sm text-gray-600">Atendemos: Lunes - Sábado · 08:00 - 18:00</p>

            <form onSubmit={handleContact} className="mt-4 space-y-3">
              <input name="name" placeholder="Tu nombre" value={booking.name} onChange={handleChange} className="w-full p-3 rounded-md border" />
              <input name="email" placeholder="Email" value={booking.email} onChange={handleChange} className="w-full p-3 rounded-md border" />
              <textarea name="notes" placeholder="Mensaje" value={booking.notes} onChange={handleChange} className="w-full p-3 rounded-md border" />
              <div>
                <button className="px-4 py-2 rounded-md bg-blue-600 text-white">Enviar mensaje</button>
              </div>
            </form>
          </div>

          <div className="p-6 bg-white rounded-2xl shadow-sm">
            <h4 className="font-semibold mb-3">Dirección</h4>
            <p className="text-sm text-gray-600">Nuestra oficina: 123 Cleaning St, London, UK</p>
            <div className="mt-4">
              {/* Map placeholder: replace with embedded Google Maps iframe when ready */}
              <div className="w-full h-40 bg-gray-100 rounded-lg flex items-center justify-center text-sm text-gray-500">Mapa aquí (iframe)</div>
            </div>
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="bg-gray-900 text-white py-6 mt-12">
        <div className="max-w-6xl mx-auto px-6 flex flex-col md:flex-row justify-between items-center gap-4">
          <div className="text-sm">© {new Date().getFullYear()} Cleaning Company — Todos los derechos reservados</div>
          <div className="text-sm">Política de privacidad · Términos · Contacto</div>
        </div>
      </footer>
    </div>
  );
}

/*
  Instrucciones rápidas para conectar pagos (Stripe):
  1) En tu servidor (Node.js/Express por ejemplo) instala stripe y crea endpoint POST /api/create-checkout-session
     que reciba los booking details y cree una Checkout Session:

     const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
     app.post('/api/create-checkout-session', async (req, res) => {
       const booking = req.body.booking;
       // calcular monto, metadata, etc.
       const session = await stripe.checkout.sessions.create({
         payment_method_types: ['card'],
         line_items: [{ price_data: { currency: 'gbp', product_data: { name: booking.serviceTitle }, unit_amount: Math.round(booking.amount * 100) }, quantity: 1 }],
         mode: 'payment',
         success_url: `${process.env.FRONTEND_URL}/success?session_id={CHECKOUT_SESSION_ID}`,
         cancel_url: `${process.env.FRONTEND_URL}/cancel`,
         metadata: { booking: JSON.stringify(booking) }
       });
       res.json({ url: session.url });
     });

  2) Protege tus claves con variables de entorno. 3) Implementa webhooks (checkout.session.completed)
     para guardar la reserva en tu base de datos y enviar confirmaciones por email.

  Despliegue sugerido: Vercel (frontend) + Vercel Serverless Functions o Heroku/Render (backend).
*/
