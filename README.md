> Tulove:
// PayPal client ID const PAYPAL_CLIENT_ID = "EEJzqGti3xsSWCOQddcWH1gHXvhIV7CbB4Tmy7m1LwdQQqoEWoVPSSWXZ90HArBoUzfrARaYoELdvDWR";

import React, { useState, useEffect } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Moon, Sun, PlayCircle, Download, Lock } from "lucide-react";

export default function EliahTulovemovies() { const [darkMode, setDarkMode] = useState(false); const [adminLoggedIn, setAdminLoggedIn] = useState(false); const [email, setEmail] = useState(""); const [password, setPassword] = useState(""); const [selectedMovie, setSelectedMovie] = useState(null); const [playbackSpeed, setPlaybackSpeed] = useState(1); const [paidMovies, setPaidMovies] = useState([]); const [movies, setMovies] = useState([ { id: 1, title: "Movie One", thumbnail: "/movie1.jpg", video: "/video1.mp4", premium: false }, { id: 2, title: "Movie Two", thumbnail: "/movie2.jpg", video: "/video2.mp4", premium: true }, ]);

useEffect(() => { if (typeof window !== "undefined") { const script = document.createElement("script"); script.src = https://www.paypal.com/sdk/js?client-id=${PAYPAL_CLIENT_ID}; script.async = true; script.onload = () => console.log("PayPal SDK loaded"); document.body.appendChild(script); } }, []);

const handleLogin = () => { if (email === "admin@eliahtulovemovies.com" && password === "admin123") { setAdminLoggedIn(true); } else { alert("Invalid credentials"); } };

const handleUpload = (e) => { e.preventDefault(); const form = e.target; const title = form.title.value; const thumbnail = form.thumbnail.value; const video = form.video.value; const premium = form.premium.checked; const newMovie = { id: movies.length + 1, title, thumbnail, video, premium }; setMovies([...movies, newMovie]); form.reset(); };

const loadPayPalButton = (movieId) => { const containerId = paypal-button-container-${movieId}; setTimeout(() => { if (window.paypal && document.getElementById(containerId)) { window.paypal.Buttons({ createOrder: (data, actions) => { return actions.order.create({ purchase_units: [{ amount: { value: "2.00" } }], }); }, onApprove: (data, actions) => { return actions.order.capture().then(() => { setPaidMovies((prev) => [...prev, movieId]); alert("Payment successful! Movie unlocked."); }); }, }).render(#${containerId}); } }, 500); };

return ( <div className={darkMode ? "bg-black text-white min-h-screen" : "bg-white text-black min-h-screen"}> <div className="flex justify-between items-center p-4 shadow"> <h1 className="text-xl font-bold">EliahTulovemovies</h1> <Button onClick={() => setDarkMode(!darkMode)} variant="outline"> {darkMode ? <Sun /> : <Moon />} </Button> </div>

{!selectedMovie ? (
    <>
      {!adminLoggedIn ? (
        <div className="max-w-md mx-auto p-4">
          <h2 className="text-lg font-semibold mb-2">Admin Login</h2>
          <input
            type="email"
            placeholder="Email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            className="w-full mb-2 p-2 border rounded"
          />
          <input
            type="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            className="w-full mb-2 p-2 border rounded"
          />
          <Button onClick={handleLogin}>Login</Button>
        </div>
      ) : (
        <div className="max-w-md mx-auto p-4">
          <h2 className="text-lg font-semibold mb-2">Upload New Movie</h2>
          <form onSubmit={handleUpload} className="space-y-2">
            <input name="title" placeholder="Movie Title" className="w-full p-2 border rounded" required />
            <input name="thumbnail" placeholder="Thumbnail URL" className="w-full p-2 border rounded" required />

> Tulove:
<input name="video" placeholder="Video URL (mp4)" className="w-full p-2 border rounded" required />
            <label className="flex items-center gap-2">
              <input type="checkbox" name="premium" /> Premium Movie
            </label>
            <Button type="submit">Upload</Button>
          </form>
        </div>
      )}

      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 p-4">
        {movies.map((movie) => (
          <Card key={movie.id} className="rounded-2xl shadow-lg">
            <img src={movie.thumbnail} alt={movie.title} className="w-full h-48 object-cover rounded-t-2xl" />
            <CardContent>
              <h2 className="text-lg font-semibold mb-2">{movie.title}</h2>
              {movie.premium && !paidMovies.includes(movie.id) ? (
                <div className="space-y-2">
                  <div id={paypal-button-container-${movie.id}}></div>
                  {loadPayPalButton(movie.id)}
                  <Button className="w-full bg-yellow-500 hover:bg-yellow-600" onClick={() => alert("Coming soon: Mobile Money")}>Pay with Mobile Money</Button>
                </div>
              ) : (
                <div className="flex flex-col gap-2">
                  <Button onClick={() => setSelectedMovie(movie)}><PlayCircle className="mr-1" /> Play</Button>
                  <a href={movie.video} download>
                    <Button variant="secondary"><Download className="mr-1" /> Download</Button>
                  </a>
                </div>
              )}
            </CardContent>
          </Card>
        ))}
      </div>
    </>
  ) : (
    <div className="p-4">
      <Button onClick={() => setSelectedMovie(null)} className="mb-4">Back to Movies</Button>
      <h2 className="text-xl font-bold mb-2">{selectedMovie.title}</h2>
      <video
        src={selectedMovie.video}
        controls
        className="w-full max-w-4xl mx-auto rounded-lg"
        playbackRate={playbackSpeed}
      ></video>
      <div className="mt-4 flex gap-2 flex-wrap">
        {[0.5, 1, 1.5, 2].map((speed) => (
          <Button
            key={speed}
            onClick={() => setPlaybackSpeed(speed)}
            variant={speed === playbackSpeed ? "default" : "outline"}
          >
            {speed}x
          </Button>
        ))}
      </div>
    </div>
  )}

  <div className="fixed bottom-4 right-4">
    <a
      href="https://api.whatsapp.com/send?phone=+256761787997&text=Movie%20requests"
      target="_blank"
      rel="noopener noreferrer"
    >
      <Button className="bg-green-500 hover:bg-green-600">WhatsApp</Button>
    </a>
  </div>
</div>

); }
