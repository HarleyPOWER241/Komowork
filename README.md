import React, { useState } from "react";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Search, Send, CheckCircle, Upload, ShieldCheck, Star, MessageCircle } from "lucide-react";
import Image from "next/image";
import PayPalButton from "@/components/PayPalButton"; // Fichier fictif pour le paiement

export default function Home() {
  const [messages, setMessages] = useState([
    { sender: "Recruteur", text: "Bonjour, nous avons bien reçu votre candidature." },
    { sender: "Candidat", text: "Merci pour votre retour !" },
  ]);
  const [newMessage, setNewMessage] = useState("");
  const [isSubscribed, setIsSubscribed] = useState(false);
  const [verifiedCandidates, setVerifiedCandidates] = useState(["Candidat1", "Candidat2"]);
  const [verifiedTrainers, setVerifiedTrainers] = useState([
    { name: "Formateur1", hourlyRate: "" },
    { name: "Formateur2", hourlyRate: "" }
  ]);
  const [files, setFiles] = useState([]);
  const [jobPost, setJobPost] = useState({ company: "", position: "", contractType: "", requirements: "" });
  const [trainerInfo, setTrainerInfo] = useState({ name: "", hourlyRate: "" });
  const [logo, setLogo] = useState("/Colorful Minimalist Social Community Logo.png");
  
  const sendMessage = () => {
    if (newMessage.trim() !== "") {
      setMessages([...messages, { sender: "Candidat", text: newMessage }]);
      setNewMessage("");
    }
  };

  const handleFileUpload = (event) => {
    setFiles([...files, ...event.target.files]);
  };

  const handleJobPostChange = (e) => {
    setJobPost({ ...jobPost, [e.target.name]: e.target.value });
  };

  const handleTrainerChange = (e) => {
    setTrainerInfo({ ...trainerInfo, [e.target.name]: e.target.value });
  };

  const handleLogoChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      setLogo(URL.createObjectURL(file));
    }
  };

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      {/* Header */}
      <header className="bg-blue-600 text-white py-4 px-6 text-center flex items-center justify-center gap-4">
        <Image src={logo} alt="KOMOWORK Logo" width={80} height={80} />
        <span className="text-2xl font-bold">KOMOWORK - Trouvez l'emploi idéal !</span>
        <input type="file" onChange={handleLogoChange} className="ml-4 text-white" />
      </header>
      
      {/* Navigation Tabs */}
      <div className="flex justify-center space-x-6 mt-4">
        <Button className="bg-blue-600 text-white">Emplois</Button>
        <Button className="bg-gray-300 text-black">Formation & Coaching</Button>
      </div>
      
      {/* Trainer Registration Section */}
      <div className="mt-10 max-w-2xl mx-auto bg-white p-6 rounded-lg shadow-lg">
        <h2 className="text-xl font-semibold text-center mb-4">Devenir Formateur</h2>
        <Input
          name="name"
          value={trainerInfo.name}
          onChange={handleTrainerChange}
          placeholder="Nom du formateur"
          className="mb-3"
        />
        <Input
          name="hourlyRate"
          value={trainerInfo.hourlyRate}
          onChange={handleTrainerChange}
          placeholder="Coût de la formation par heure (FCFA)"
          className="mb-3"
        />
        <Button className="bg-blue-600 text-white w-full">S'inscrire</Button>
      </div>
      
      {/* Verified Badge Section for Trainers */}
      <div className="mt-10 max-w-2xl mx-auto">
        <h2 className="text-xl font-semibold text-center mb-4">Formateurs Vérifiés</h2>
        {verifiedTrainers.map((trainer, index) => (
          <div key={index} className="flex items-center justify-between bg-white p-4 mb-2 rounded-lg shadow">
            <span>{trainer.name} - {trainer.hourlyRate} FCFA/h</span>
            <div className="flex space-x-2">
              <Star className="text-yellow-500 h-6 w-6" />
              <MessageCircle className="text-blue-500 h-6 w-6 cursor-pointer" title="Envoyer un message" />
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}
