"use client";

import { useState } from "react";
import { MapContainer, TileLayer, CircleMarker, Popup, useMapEvents } from "react-leaflet";
import "leaflet/dist/leaflet.css";

// Mock meteor data
const sightings = [
  { id: 1, lat: 19.076, lon: 72.8777, mag: -2.1, label: "Mumbai, IN" },
  { id: 2, lat: 28.6139, lon: 77.209, mag: -1.3, label: "Delhi, IN" },
  { id: 3, lat: 34.0522, lon: -118.2437, mag: -3.0, label: "Los Angeles, US" },
  { id: 4, lat: 51.5072, lon: -0.1276, mag: -2.6, label: "London, UK" },
  { id: 5, lat: -33.8688, lon: 151.2093, mag: -1.8, label: "Sydney, AU" },
  { id: 6, lat: 35.6762, lon: 139.6503, mag: -0.9, label: "Tokyo, JP" },
];

export default function MeteorMadness() {
  const [selected, setSelected] = useState(null);

  function ClickHandler() {
    useMapEvents({
      click(e) {
        setSelected([e.latlng.lat, e.latlng.lng]);
      },
    });
    return null;
  }

  return (
    <div style={{ minHeight: "100vh", background: "#ffffff", padding: "20px" }}>
      <h1 style={{ textAlign: "center", fontSize: "28px", fontWeight: "bold" }}>
        NASA Space Apps 2025 — Meteor Madness 🌍
      </h1>

   <div style={{ marginTop: "20px", height: "500px", borderRadius: "12px", overflow: "hidden", border: "1px solid #ddd" }}>
        <MapContainer
          center={[20, 0]}
          zoom={2}
          scrollWheelZoom={true}
          style={{ height: "100%", width: "100%" }}
        >
          <TileLayer
            url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
            attribution="&copy; OpenStreetMap contributors"
          />

   {/* Existing meteor detections */}
          {sightings.map((p) => (
            <CircleMarker
              key={p.id}
              center={[p.lat, p.lon]}
              radius={6 + Math.max(0, -p.mag)}
              pathOptions={{ color: "#2563eb", fillOpacity: 0.6 }}
            >
              <Popup>
                {p.label} <br /> Magnitude: {p.mag}
              </Popup>
            </CircleMarker>
          ))}

   {/* Selected location */}
          {selected && (
            <CircleMarker
              center={selected}
              radius={8}
              pathOptions={{ color: "#10b981", fillOpacity: 0.9 }}
            >
              <Popup>
                Selected Location:<br />
                {selected[0].toFixed(4)}, {selected[1].toFixed(4)}
              </Popup>
            </CircleMarker>
          )}

   <ClickHandler />
        </MapContainer>
      </div>

   <div style={{ marginTop: "15px", textAlign: "center", fontSize: "16px" }}>
        {selected ? (
          <p>
            Selected Coordinates: <b>{selected[0].toFixed(4)}</b>, <b>{selected[1].toFixed(4)}</b>
          </p>
        ) : (
          <p>Click anywhere on the map to select a meteor location.</p>
        )}
      </div>
    </div>
  );
}
