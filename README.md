# Digiplus

from flask import Flask, request, jsonify
from datetime import datetime

app = Flask(__name__)

# Simulated database of SIM cards
sim_database = {
    "123456789012345": {"status": "inactive", "activation_date": None},
    "987654321098765": {"status": "inactive", "activation_date": None},
}

@app.route('/activate', methods=['POST'])
def activate_sim():
    # Get the SIM number from the request
    data = request.get_json()
    
    if not data or 'sim_number' not in data:
        return jsonify({"error": "SIM number is required"}), 400

    sim_number = data['sim_number']

    # Check if the SIM number exists in the database
    if sim_number not in sim_database:
        return jsonify({"error": "SIM number not found"}), 404

    # Check if the SIM is already active
    if sim_database[sim_number]['status'] == 'active':
        return jsonify({"error": "SIM card is already active"}), 400

    # Activate the SIM card
    sim_database[sim_number]['status'] = 'active'
    sim_database[sim_number]['activation_date'] = datetime.now().isoformat()

    return jsonify({"message": "SIM card activated successfully", "sim_number": sim_number}), 200

if __name__ == '__main__':
    app.run(debug=True)

